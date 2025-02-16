---
comments : true
---

本文主要介绍如何在 nvim 中配置使用 ccls。

## 安装与配置

本文使用 Lazy vim 插件管理器，首先安装 LSP 管理插件：

```
... -- 省略其他行
require("lazy").setup({
	-- LSP manager
	"williamboman/mason.nvim",
	"williamboman/mason-lspconfig.nvim",
	"neovim/nvim-lspconfig",
    ... -- 省略其他行
})
```

其中 mason 是方便各种 LSP 安装和配置的，nvim-lspconfig 则是负责与 LSP 进行交互，而 mason-lspconfig 则是将两者联系起来。

我们一般在安装 LSP 时，可以先去 mason 的列表里面找一下，如果有就直接填空就自动安装好了（例如 clangd）。但不幸的是目前（2024/11/17）没有支持 ccls，因此我们需要手动安装 ccls。

使用 Ubuntu 的朋友可以参考我的上一篇文章来安装 ccls。使用其他系统的朋友也可以借鉴一下，源码编译步骤都一样，提前声明可能会遇到一些坑。

nvim-lspconfig 的配置如下：

```lua
local lspconfig = require('lspconfig')
local util = require 'lspconfig.util'
lspconfig.ccls.setup {
  on_attach = on_attach,
  -- 开启单文件支持
  single_file_support = true,
  -- 根目录获取规则
  root_dir = function(fname)
    return util.root_pattern('compile_commands.json', '.ccls')(fname) or util.find_git_ancestor(fname) or vim.fn.getcwd()
  end,
  -- 初始化参数
  init_options = {
    compilationDatabaseDirectory = "",
    cache = {
      directory = ".ccls-cache"
    },
    index = {
      threads = 32;
    },
    -- 需要读者定制化添加，有一些系统库并没有被 clang 默认索引
    -- 可通过 clang++ -v -E -x c++ - 查看默认的 include 路径
    clang = {
      extraArgs = { 
        "-I/usr/include", 
        "-I/usr/local/include", 
        "-I/usr/include/c++/13",
      },
      resourceDir = ""
    } 
  }
}
```

官方配置参考：[nvim-lspconfig/doc/configs.md at master · neovim/nvim-lspconfig](https://github.com/neovim/nvim-lspconfig/blob/master/doc/configs.md#ccls)

除了编写 .ccls 文件或者 compile_commands.json 文件，还可以通过添加命令行参数和初始化选项这两种方式能够使 ccls 正常工作以外。对于大项目而言（通过 cmake 或者 makefile），生成 compile_commands.json 是比较方便的，我们在后文具体讲讲如何生成。

但对于单文件来说，添加初始化参数是最方便的形式，我们在上面的配置中开启了单文件支持，并添加了系统 include 路径，最重要的，我们修改了 root_dir 的获取方式，新增了 vim.fn.getcwd() 来获取 nvim 当前的打开路径，在此之前，lspconfig 只支持查找 .ccls 和 compile_commands.json 的所在目录以及当前 git 文件所在目录，而对于一个单文件来说这些文件都没有。lspconfig ccls 还支持更多的参数配置，可参考：[Customization · MaskRay/ccls Wiki](https://github.com/MaskRay/ccls/wiki/Customization#initialization-options)。

在 Ubuntu 上使用 `ccls` 时，`ccls` 会根据项目的 **编译数据库**（`compile_commands.json`）来生成索引，并将结果存储在 `ccls-cache` 目录中。如果你想自动生成 `ccls-cache`，需要完成以下步骤：

生成 compile_commands.json 文件

`ccls` 需要通过 `compile_commands.json` 来了解项目的编译信息。以下是生成这个文件的方法：

#### **使用 CMake 生成**

1. 在项目根目录下运行：

   ```bash
   cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON <path/to/your/project>
   ```

2. `compile_commands.json` 将生成在项目的构建目录（通常是 `build/`）。

3. 如果需要将其复制到项目根目录：

   ```bash
   cp build/compile_commands.json .
   ```

#### **其他构建工具生成**

- **Makefile 项目**：可以使用工具如 [Bear](https://github.com/rizsotto/Bear) 生成：

  ```bash
  sudo apt install bear
  bear -- make <your_project_name>
  ```

  这将在项目根目录生成 `compile_commands.json`。

- **手动生成**：如果项目简单，也可以手动编写一个 `compile_commands.json`，但这不推荐，因为容易出错。

配置好的样子：

![image-20241117190821333](https://blog-1256878123.cos.ap-nanjing.myqcloud.com/img/202411171908363.png)

## 参考

- [Clangd 配置，利用Cmake/Makefile生成json - 知乎](https://zhuanlan.zhihu.com/p/627004117)
- [Customization · MaskRay/ccls Wiki](https://github.com/MaskRay/ccls/wiki/Customization#initialization-options)