---
comments: true
---

# neovim 配置

> 前言
>
> 本文主要介绍 neovim 的安装和推荐插件的配置和基本使用方法。写作本文，一是为了配置 neovim 时有所参考，二是为了更好的宣传 neovim。
>
> 特此说明，在安装插件时可能会遇到各种各样的问题，一些常见的排查角度是：
> - neovim 版本是否符合要求？
> - 插件所依赖的环境是否安装成功？
> - glibc 版本是否太低？
> - 插件配置的代码与否有语法错误、路径错误？

## install

```shell
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux64.tar.gz 
sudo rm -rf /opt/nvim
sudo tar -C /opt -xzf nvim-linux64.tar.gz
export PATH="$PATH:/opt/nvim-linux64/bin" # in .bashrc or .zshrc
```

最后一句 shell 命令将 nvim 的路径添加到 PATH 当中，要记得把命令添加到 .bashrc 中以长期生效。

然后你就可以使用 nvim 来启动 nvim 了。

## 配置方法

我的 nvim 配置在 [https://github.com/kpole/dotfiles](https://github.com/kpole/dotfiles) 的 .config 目录中。目录结构如下：
```
├── init.lua                            -- nvim 初始化
├── lazy-lock.json
└── lua                                 -- 关于 nvim 的配置
    ├── colorscheme.lua                 -- 高亮配置
    ├── config                          -- 插件配置
    │   ├── bufferline.lua
    │   ├── catppuccin.lua
    │   ├── comment.lua
    │   ├── log_highlight.lua
    │   ├── lualine.lua
    │   ├── nvim-cmp.lua
    │   ├── nvim-tree.lua
    │   ├── telescope.lua
    │   ├── toggleterm.lua
    │   └── _treesitter.lua
    ├── keymaps.lua                     -- 全局快捷键配置
    ├── lsp.lua                         -- LSP 配置
    ├── options.lua                     -- 全局配置
    └── plugins.lua                     -- 插件列表
```

每次安装插件，需要在 plugins.lua 中添加插件。如果插件需要详细配置，可以在 config 中添加相应的文件（文件名可自定义，但注意有些插件的配置文件名不能与插件同名，否则引入插件时会有问题），并在 plugins.lua 中引入。

该文件目录参考来自：[https://www.cnblogs.com/youngxhui/p/17730419.html](https://www.cnblogs.com/youngxhui/p/17730419.html)。这篇文章写的很好，如有需要，建议读者阅读。


## 插件

插件可以使你的 nvim 如虎添翼。

下表罗列出本文推荐安装的几款插件。

| 插件名称        | 作用               |
| :-------------- | :----------------- |
| nvim-tree       | 文件树目录         |
| bufferline      | 文件 tab 栏        |
| Telescope       | 文件查找、字段查找 |
| auto-pairs      | 括号匹配           |
| nvim-treesitter | 关键字匹配、高亮   |

### nvim-tree

nvim-tree 是一个用于 Neovim 的文件资源管理器插件，提供树状目录视图，便于文件浏览和操作。它支持文件创建、删除、重命名、拖放等功能，并集成了 Git 状态显示。

下载链接：[nvim-tree/nvim-tree.lua: A file explorer tree for neovim written in lua (github.com)](https://github.com/nvim-tree/nvim-tree.lua)

API列表：[nvim-tree.lua/doc/nvim-tree-lua.txt at master · nvim-tree/nvim-tree.lua (github.com)](https://github.com/nvim-tree/nvim-tree.lua/blob/master/doc/nvim-tree-lua.txt)

使用 :NvimTreeOpen 可以调出文件树窗口，使用 g? 可以展示快捷键帮助

在 lua/keymaps.lua 进行配置，通过 `<leader>e` 可以开启或关闭文件树，通过 `<leader>t` 可以将光标聚焦于文件树，通过 `<leader>ff` 来定位当前文件在文件树中的位置，

一般来说，`<leader>` 默认是 `\`。

```
vim.keymap.set("n", "<leader>e", ":NvimTreeToggle<CR>", opts)
vim.keymap.set("n", "<leader>t", ":NvimTreeFocus<CR>", opts)
vim.keymap.set("n", "<leader>ff", ":NvimTreeFindFile<CR>", opts)
vim.keymap.set("n", "<leader>c", ":NvimTreeFindFileToggle<CR>", opts)
```

### bufferline
### Telescope

这个插件用来搜索文件、搜索某个字段在哪些文件中出现。

下载链接：[https://github.com/nvim-telescope/telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)

常用的功能主要是三个：
1. 通过文件名查找文件：Telescope find_files
2. 通过字段来查找在哪些文件中出现: Telescope live_grep
3. 查看 buffers，即已经打开的文件列表 buffer：Telescope buffers

如果查看 buffers 时想按照打开顺序对文件进行排序（就像 vscode 中那样），可以进行设置：

```lua
local status, telescope = pcall(require, "telescope")
telescope.setup{
  pickers = {
    buffers = {
      ignore_current_buffer = true,
      sort_lastused = true,
    },
  },
}
```

在选择文件时常用的一些快捷键：

```
# 选择相关
<C-n>/<Down>    Next item
<C-p>/<Up>      Previous item
j/k             Next/Previous (in normal mode)
H/M/L	          Select High/Middle/Low (in normal mode)
gg/G	          Select the first/last item (in normal mode)

# 打开文件相关
<CR>	          Confirm selection
<C-x>	          Go to file selection as a split
<C-v>	          Go to file selection as a vsplit
<C-t>	          Go to a file in a new tab

# 阅览
<C-u>	          Scroll up in preview window
<C-d>	          Scroll down in preview window

# 其他
?	              Show mappings for picker actions (normal mode)
<C-c>	          Close telescope (insert mode)
<Esc>	          Close telescope (in normal mode)        
```

### auto-pairs
[jiangmiao/auto-pairs: Vim plugin, insert or delete brackets, parens, quotes in pair (github.com)](https://github.com/jiangmiao/auto-pairs)

自动插入、删除括号、引号

### Nvim-Treesitter

Nvim-Treesitter 利用树结构解析和语法树生成技术，为 nvim 提供更精准和高效的代码高亮、代码折叠、代码导航功能，支持多种变成语言，增强代码编辑体验。

下载链接：[nvim-treesitter/nvim-treesitter: Nvim Treesitter configurations and abstraction layer (github.com)](https://github.com/nvim-treesitter/nvim-treesitter)

在安装不同语言的 treesitter 时，可以设置不同的 compilers。例如使用 TSInstall cpp 时，需要将 compilers 设置为 clang。如果使用 clang++ 编译，则会遇到报错。

```cpp
require 'nvim-treesitter.install'.compilers = { 'clang++'}
```

使用 :checkhealth nvim-treesitter 检查是否安装成功。

### Nvim-comment

在 vscode 中，可以使用 `cmd+"` 来快速注释代码，那么在 nvim 中，可以通过安装 nvim-comment 插件实现快速进行注释。

下载链接：[terrortylor/nvim-comment: A comment toggler for Neovim, written in Lua (github.com)](https://github.com/terrortylor/nvim-comment)

直接使用命令进行注释：

+ `CommentToggle` 注释/取消注释当前行
+ `67,69CommentToggle` 注释/取消注释第 67 到第 69 行
+ `'<,'>CommentToggle` 注释/取消注释 visual 模式下选择的代码块

或者使用快捷指令：

+ `gcc` 注释/取消注释当前行
+ `gc{motion}` comment/uncomment selection defined by a motion (as lines are commented, any comment toggling actions will default to a linewise):
    - `gcip` 注释/取消注释当前段落
    - `gc4j` 注释/取消注释当前行以及下面的 4 行

### Which-key


[folke/which-key.nvim: 💥 Create key bindings that stick. WhichKey helps you remember your Neovim keymaps, by showing available keybindings in a popup as you type. (github.com)](https://github.com/folke/which-key.nvim)



### 主题插件

nvim 可用的主题有很多，可以从这里自选：[trending colorschemes | vimcolorschemes](https://vimcolorschemes.com/i/trending)


## LSP 配置
lspconfig

+ [d 跳至上一个有错误的地方
+ ]d 跳至下一个有错误的地方

nvim-cmp：[https://github.com/hrsh7th/nvim-cmp](https://github.com/hrsh7th/nvim-cmp)

[详解nvim内建LSP体系与基于nvim-cmp的代码补全体系-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2346026)

mason：[https://github.com/williamboman/mason-lspconfig.nvim](https://github.com/williamboman/mason-lspconfig.nvim)

mason-lspconfig：[https://github.com/williamboman/mason-lspconfig.nvim](https://github.com/williamboman/mason-lspconfig.nvim?)

nvim-lspconfig：[https://github.com/neovim/nvim-lspconfig/tree/master](https://github.com/neovim/nvim-lspconfig/tree/master)

nvim-lspconfig 支持的 LSP：[https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md)



## 剪切板配置
安装 xclip：[Releases · astrand/xclip (github.com)](https://github.com/astrand/xclip)

```shell
./bootstrap
./configure
make
make install
```

过程中如果报错：configure: error: *** X11/Xmu/Atoms.h is missing ***

则需要安装 libXmu-devel：sudo yum install libXmu-devel

Mac 端安装 [How to install XQuartz on macOS for SSH X11 forwarding - nixCraft (cyberciti.biz)](https://www.cyberciti.biz/faq/apple-osx-mountain-lion-mavericks-install-xquartz-server/)

注意要安装 xuath。

然后，需要将服务器的 sshd_config 中 X11Forwarding 打开，然后重启 ssh 服务（很危险，执行之前最好保证配置项正确）：

```shell
$sudo cat /etc/ssh/sshd_config | grep 'X11'
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost yes
```

然后将客户端的 ssh_config 中 ForwardX11 打开：

```shell
    ForwardX11 yes # 9.19 add：看起来不加也可以
#    X11Forwarding yes
    XAuthLocation /opt/X11/bin/xauth
```

接着，修改 nvim 配置：

```shell
-- 这里是 vim 配置
let g:clipboard = {
    \ 'name': 'xclip',
    \ 'copy': {
    \ '+': 'xclip -selection clipboard',
    \ '*': 'xclip -selection clipboard',
    \ },
    \ 'paste': {
    \ '+': 'xclip -selection clipboard -o',
    \ '*': 'xclip -selection clipboard -o',
    \ },
    \ }
-- 这里是 lua 配置脚本   
vim.g.clipboard = {
    name = 'xclip',
    copy = {
        ['+'] = 'xclip -selection clipboard',
        ['*'] = 'xclip -selection clipboard',
    },
    paste = {
        ['+'] = 'xclip -selection clipboard -o',
        ['*'] = 'xclip -selection clipboard -o',
    },
}
```

启动 mac 上的 Xquartz，并保证其设置 > 安全性 > 允许从网络客户端链接设置为 true。

安装成功后，控制台查看 DISPLAY 环境变量，可以正常输出。

```shell
$echo $DISPLAY
localhost:12.0
```

### 终端
:terminal

:vsplit | terminal

:split | terminal

退出 terminal 的 insert 模式：ctrl+\ 再按下 ctrl + n 即可退出。因为 esc 在终端的行为和文本编辑模式中有所不同，esc 可能会被视为发送字符给终端，而不是用于切换模式。

终端插件：[akinsho/toggleterm.nvim: A neovim lua plugin to help easily manage multiple terminal windows (github.com)](https://github.com/akinsho/toggleterm.nvim)

使用方法  
ToggleTerm size=40 dir=~/.config/nvim direction=horizontal name=desktop

ToggleTermToggleAll 打开之前所有的 term

TermExec

### 
## 参考资料
+ Lazy 文档：[https://lazy.folke.io/usage](https://lazy.folke.io/usage)
+ Lua 语法：[https://learnxinyminutes.com/docs/lua/](https://learnxinyminutes.com/docs/lua/)
+ 推荐的配置：[https://github.com/ayamir/nvimdots](https://github.com/ayamir/nvimdots)
+ 插件配置教程：[neovim入门指南(二)：常用插件 - youngxhui - 博客园 (cnblogs.com)](https://www.cnblogs.com/youngxhui/p/17730417.html)
+ [SmithJson/nvim: IDE VIM Configuration (github.com)](https://github.com/SmithJson/nvim)
+ [https://martinlwx.github.io/zh-cn/config-neovim-from-scratch](https://martinlwx.github.io/zh-cn/config-neovim-from-scratch)
+ [https://zhuanlan.zhihu.com/p/382092667](https://zhuanlan.zhihu.com/p/382092667)
+ neovim doc：[https://neovim.io/doc/](https://neovim.io/doc/)
+ Node：[https://nodejs.org/en/download/package-manager](https://nodejs.org/en/download/package-manager)