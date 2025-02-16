## 前言

nvimdots，[ayamir/nvimdots: A well configured and structured Neovim.](https://github.com/ayamir/nvimdots) 是一套易用性强的 nvim 配置框架，一键安装即用，并且支持用户自定义快捷键、插件配置，总体来说是一款非常不错的 nvim 皮肤。

博主本人两年前第一次接触 nvim 时就找到过这个配置框架，但由于当时还没有将 vim/nvim 当做主力编辑器，不会 lua，也不懂如何配置 nvim，就没有耐心去详细了解这个庞大而全面的框架。两年之后开始工作，打算重拾起来 nvim，自己从头配置了一套能用的配置，勉强应付平时工作。但耐不住突然遇到一个需求，发现还没配置，如果要放下手头工作去查 nvim 如何配置，少则半个钟，多则一两个钟就没了。也可能是我有先天踩坑圣体，总是能踩到一些莫名其妙的坑。于是在用了两个多月自己的配置之后，决定再试一试这个框架。用过之后，发现还是别人的好用。

但是对于这个框架而言，上手难度并不低，这里的难度并不是指理解的难度极值，而是指整个框架的复杂度，首先这个框架有 92 个插件，然后还有约 7000 行配置代码。如果是 nvim 老手就还好，对于新手而言需要足够的心理准备。想要对整个框架足够了解，需要充分的时间，我陆陆续续花了两个星期，连带每天下班后的几个小时再加上整个周末，才把这些插件的基本用法梳理完，并整理出了这一套指导手册。希望对读者们有所帮助。

## 安装

nvimdots 提供了方便快捷的安装脚本，可以直接根据脚本进行安装。不过在此之前要保证你的机器上安装好了 nvim。

安装成功后，配置就放在了 `~/.config/nvim` 目录中，你可以在命令行直接输入 `nvim` 打开了，不一会就可以看到很多插件开始安装。 

注意，本框架虽然是开箱即用，但由于每个人的环境不同，并且框架本身包含的某些插件还需要一些额外的系统环境，所以会出现各种各样奇奇怪怪的问题。本文会从配置的角度告诉你哪些插件是做什么，它需要如何才能正常工作。

## 文件结构

我们进入到 `~/.config/nvim` 中，里面的 `init.lua` 就是配置入口，它主要引入了 lua 文件夹的配置文件。因此我们主要关注 lua 中的文件就好了。

> 本框架都是用的 lua 语言，但看到框架的基本结构并不需要 lua 语言基础。不过读者如果有兴趣了解，可以参考：[Lua 5.4 中文参考手册](https://atom-l.github.io/lua5.4-manual-zh/1.html)

lua 的文件结构如下：

```
.
├── core						
├── keymap
├── modules
│   ├── configs
│   ├── plugins
│   └── utils
└── user_template
    ├── configs
    ├── event.lua
    ├── keymap
    ├── options.lua
    ├── plugins
    └── settings.lua
```

core 是配置文件的核心，它负责整个配置流程，调用其他文件夹中的一些配置。keymap 文件夹包含了所有的快捷键设置，modules 包含了所有插件的配置，而 user_template 是用户配置的模板，在安装好 nvim 之后，安装脚本会根据 user_template 生成一套用户配置，用户根据自己的需求更改用户配置后就可生效。将用户配置和全局配置隔离开，可以避免 nvimdots 代码仓库更新带来处理代码冲突的负担。

我们首先讲解 core 文件夹，再讲解 modules 文件夹，在讲解 modules 的同时可能会附带讲解对应 keymap 中的一些快捷键配置。

由于 core 的结构比较简单，所以就直接放在本节了，core 是配置流程的入口，结构如下：

```
.
├── event.lua
├── global.lua
├── init.lua
├── mapping.lua
├── options.lua
├── pack.lua
└── settings.lua
```

其中 init.lua 就是配置入口了，我们在全局的 init.lua 中直接 `require("core")` 后，就会执行它。

init.lua 中有很多初始化的动作，都被纳入 load_core() 函数中。在这个函数中 require 了其他四个文件：

```lua
local load_core = function()
	createdir()
	disable_distribution_plugins()
	leader_map()

	gui_config()
	neovide_config()
	clipboard_config()
	shell_config()

	require("core.options")
	require("core.mapping")
	require("core.event")
	require("core.pack")
	require("keymap")

	local colorscheme = settings.colorscheme
	local background = settings.background
	vim.api.nvim_command("set background=" .. background)
	vim.api.nvim_command("colorscheme " .. colorscheme)
end

load_core()
```

我们需要知道：

- 关于 nvim 本身的一些选项配置，放在 options.lua 中
- 关于 nvim 的一些按键映射，放在 mapping.lua 中
- 关于 nvimdots 的配置选项，放在 settings.lua 中

settings.lua 中加了详尽的英文注释，标识了每个配置选项有什么作用，我们后面主要就是修改这里的配置选项。

## 插件列表

这个框架我认为最主要的部分就是这些插件，如果我们自己去收集会花很多精力，但框架已经帮我们做了很多事情，我们现在只需要对这些插件有个基本了解，学会如何使用就可以了。

我把插件分为 6 类，分别是 LSP、代码补全、编辑器相关、语言相关、工具、UI。

### **LSP 与代码补全**

| 插件名称                                                     | 功能说明                                         |
| ------------------------------------------------------------ | ------------------------------------------------ |
| **[neovim/nvim-lspconfig](https://github.com/neovim/nvim-lspconfig)** | 负责配置、连接和使用不同的 LSP 服务器            |
| **[williamboman/mason.nvim](https://github.com/williamboman/mason.nvim)** | 管理 LSP、DAP、格式化工具和 linters 的包管理工具 |
| **[williamboman/mason-lspconfig.nvim](https://github.com/williamboman/mason-lspconfig.nvim)** | 将 `mason.nvim` 与 `nvim-lspconfig` 集成         |
| **[folke/neoconf.nvim](https://github.com/folke/neoconf.nvim)** | 管理全局和项目本地的 LSP 设置                    |
| **[Jint-lzxy/lsp_signature.nvim](https://github.com/ray-x/lsp_signature.nvim)** | 显示函数参数签名                                 |
| **[glepnir/lspsaga.nvim](https://github.com/glepnir/lspsaga.nvim)** | 提供更强大的 LSP 功能，例如跳转、诊断等          |
| **[nvim-tree/nvim-web-devicons](https://github.com/nvim-tree/nvim-web-devicons)** | 提供 Nerd Font 图标支持                          |
| **[stevearc/aerial.nvim](https://github.com/stevearc/aerial.nvim)** | 提供代码大纲窗口，便于快速跳转和浏览代码         |
| **[DNLHC/glance.nvim](https://github.com/DNLHC/glance.nvim)** | 在一个地方预览、导航和编辑 LSP 位置              |
| **[joechrisellis/lsp-format-modifications.nvim](https://github.com/joechrisellis/lsp-format-modifications.nvim)** | 部分格式化已修改的代码                           |
| **[nvimtools/none-ls.nvim](https://github.com/nvimtools/none-ls.nvim)** | 使用 Neovim 作为语言服务器 via Lua               |
| **[jay-babu/mason-null-ls.nvim](https://github.com/jay-babu/mason-null-ls.nvim)** | 将 `null-ls` 与 `mason.nvim` 集成                |

LSP（Language Server Protocol）是一种协议，用于在代码编辑器（nvim、vscode等）与语言服务器之间通信，被 vscode 首次提出，其目的是为不同的语言提供统一的智能特性：代码补全、语法检查、跳转定义、重构等等。nvim 生态提供了强大的插件来帮助我们使用 LSP。

其中，nvim-lspconfig 用来负责配置、连接和使用不同的 LSP 服务器，是最核心的插件。mason.nvim 是一个包管理工具，方便我们下载安装指定的 LSP，而 mason-lspconfig 则是将 mason 安装的 LSP 与 lspconfig 协调工作起来，起到一个协调作用，因为 lspconfig 中的许多配置需要应用到 mason 安装的 LSP 上面。

例如，settings.lua 中的 lsp_deps 即指定了要使用的 lsp，包括 bashls、clangd 等等。

另外还有一个插件 none-ls.nvim，它的作用是将代码格式化器、诊断工具等等非 LSP 抽象成 LSP 提供给 nvim 使用，而 mason-null-ls 则是用于简化安装 none-ls.nvim 支持的格式化器、诊断工具等等。例如 clang_format、gofumpt 等等。

好，主要的插件说完了，接下来说一下其他插件。

- neoconf，当我们希望针对不同的项目有不同的 nvim 配置时，就需要用到这个插件，它可以统一管理全局、局部的 nvim 配置，并且支持从已有的 vscode 中导入配置。
- lsp_signature.nvim，这个插件帮助你在填充函数参数时进行提示。因为有些函数的参数很多，你可能不知道写到哪个了。
- lspsaga，提供更强大的功能，例如跳转和诊断等等。在  keymap/completion.lua 中可以看到相关的快捷键设置，例如 `g[` 可以快速跳转到上一个诊断信息，`gD` 快速跳转到函数的定义处等等。
- aerial.nvim，提供代码大纲窗口，keymap/completion.lua 中设置了 `go` 打开。
- glance.nvim，LSP 导航，可以查看定义、实现和引用的位置。
- lsp-format-modifications，支持只格式化修改的那部分代码，例如在一个多人协作的项目中，老代码或者别人写的代码并没有格式化，而你将整个文件都格式化可能会带来代码 review 负担，因此可以指定只 format 你更新的那部分，settings.lua 中 format_modifications_only 选项可以控制是否开启这个功能。

------

### **自动补全与 Snippet**

#### **自动补全**

| 插件名称                                                     | 功能说明                             |
| ------------------------------------------------------------ | ------------------------------------ |
| **[hrsh7th/nvim-cmp](https://github.com/hrsh7th/nvim-cmp)**  | Neovim 的自动补全插件                |
| **[L3MON4D3/LuaSnip](https://github.com/L3MON4D3/LuaSnip)**  | `nvim-cmp` 的 snippet 引擎           |
| **[rafamadriz/friendly-snippets](https://github.com/rafamadriz/friendly-snippets)** | `LuaSnip` 的 snippets 来源           |
| **[lukas-reineke/cmp-under-comparator](https://github.com/lukas-reineke/cmp-under-comparator)** | 更好的补全排序，优先显示带下划线的项 |
| **[saadparwaiz1/cmp_luasnip](https://github.com/saadparwaiz1/cmp_luasnip)** | `nvim-cmp` 的 `LuaSnip` 来源         |
| **[hrsh7th/cmp-nvim-lsp](https://github.com/hrsh7th/cmp-nvim-lsp)** | `nvim-cmp` 的 LSP 来源               |
| **[hrsh7th/cmp-nvim-lua](https://github.com/hrsh7th/cmp-nvim-lua)** | `nvim-cmp` 的 Lua 来源               |
| **[andersevenrud/cmp-tmux](https://github.com/andersevenrud/cmp-tmux)** | `nvim-cmp` 的 Tmux 来源              |
| **[hrsh7th/cmp-path](https://github.com/hrsh7th/cmp-path)**  | `nvim-cmp` 的路径来源                |
| **[f3fora/cmp-spell](https://github.com/f3fora/cmp-spell)**  | `nvim-cmp` 的拼写来源                |
| **[hrsh7th/cmp-buffer](https://github.com/hrsh7th/cmp-buffer)** | `nvim-cmp` 的 buffer 来源            |
| **[kdheepak/cmp-latex-symbols](https://github.com/kdheepak/cmp-latex-symbols)** | `nvim-cmp` 的 LaTeX 符号来源         |
| **[ray-x/cmp-treesitter](https://github.com/ray-x/cmp-treesitter)** | `nvim-cmp` 的 Treesitter 来源        |



#### **Copilot 支持**

| 插件名称                                                     | 功能说明                   |
| ------------------------------------------------------------ | -------------------------- |
| **[zbirenbaum/copilot.lua](https://github.com/zbirenbaum/copilot.lua)** | Copilot 的 Lua 移植版      |
| **[zbirenbaum/copilot-cmp](https://github.com/zbirenbaum/copilot-cmp)** | `nvim-cmp` 的 Copilot 来源 |

nvim-cmp 是一个补全引擎，可以从多个 source 获取补全提示供用户选择，例如 LSP、snippets 等等。

LuaSnip 可以让用户自定义各种语言的代码片段，帮助提升编码效率。

friendly-snippets 这个插件提供了多种语言常用的一些 snippets。

cmp-under-comparator 提供 cmp 展示选项的排序功能，优先显示带下划线的项。

cmp_luasnip 是 luasnip 的增强插件。

cmp-nvim-lsp、cmp-nvim-lua、cmp-tmux、cmp-path、cmp-spell，cmp-buffer，cmp-latex-symbols、cmp-treesitter 等等都是 cmp 的来源插件。



------

### Editor

| 插件名称                                                     | 功能说明                                         |
| ------------------------------------------------------------ | ------------------------------------------------ |
| **[olimorris/persisted.nvim](https://github.com/olimorris/persisted.nvim)** | 会话管理                                         |
| **[m4xshen/autoclose.nvim](https://github.com/m4xshen/autoclose.nvim)** | 自动配对与闭合括号                               |
| **[LunarVim/bigfile.nvim](https://github.com/LunarVim/bigfile.nvim)** | 提供大文件支持                                   |
| **[ojroques/nvim-bufdel](https://github.com/ojroques/nvim-bufdel)** | 安全关闭缓冲区，配合 `bufferline.nvim` 使用      |
| **[folke/flash.nvim](https://github.com/folke/flash.nvim)**  | 使用搜索标签和 Treesitter 集成来增强代码导航     |
| **[numToStr/Comment.nvim](https://github.com/numToStr/Comment.nvim)** | 提供更好的注释功能                               |
| **[sindrets/diffview.nvim](https://github.com/sindrets/diffview.nvim)** | Git diff 视图                                    |
| **[echasnovski/mini.align](https://github.com/echasnovski/mini.align)** | 交互式对齐文本                                   |
| **[smoka7/hop.nvim](https://github.com/smoka7/hop.nvim)**    | 更强大的跳跃移动功能                             |
| **[tzachar/local-highlight.nvim](https://github.com/tzachar/local-highlight.nvim)** | 高亮当前光标下的单词                             |
| **[brenoprata10/nvim-highlight-colors](https://github.com/brenoprata10/nvim-highlight-colors)** | 高亮显示颜色                                     |
| **[romainl/vim-cool](https://github.com/romainl/vim-cool)**  | 自动清除搜索高亮                                 |
| **[lambdalisue/suda.vim](https://github.com/lambdalisue/suda.vim)** | 允许以非特权会话的权限编辑文件                   |
| **[tpope/vim-sleuth](https://github.com/tpope/vim-sleuth)**  | 自动根据当前文件调整 `shiftwidth` 和 `expandtab` |
| **[nvim-pack/nvim-spectre](https://github.com/nvim-pack/nvim-spectre)** | 基于项目的查找和替换                             |
| **[mrjones2014/smart-splits.nvim](https://github.com/mrjones2014/smart-splits.nvim)** | 合并 Neovim 和终端分割的导航与调整大小           |
| **[nvim-treesitter/nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)** | 强大的代码高亮插件                               |
| **[andymass/vim-matchup](https://github.com/andymass/vim-matchup)** | 增强的 `%` 匹配功能                              |
| **[mfussenegger/nvim-treehopper](https://github.com/mfussenegger/nvim-treehopper)** | 选择文本对象，如同 `hop.nvim` 一样               |
| **[nvim-treesitter/nvim-treesitter-textobjects](https://github.com/nvim-treesitter/nvim-treesitter-textobjects)** | 在文本对象之间跳转                               |
| **[windwp/nvim-ts-autotag](https://github.com/windwp/nvim-ts-autotag)** | 更快的 `vim-closetag`                            |
| **[nvim-treesitter-context](https://github.com/nvim-treesitter/nvim-treesitter-context)** | 显示当前可见缓冲区的上下文                       |
| **[JoosepAlviste/nvim-ts-context-commentstring](https://github.com/JoosepAlviste/nvim-ts-context-commentstring)** | 基于上下文的注释功能                             |

------

#### diffview

diffview 可以查看 git diff 的视图。注意这个插件目前需要 git version 在 2.31.0 及以上。

keymap/editor.lua 中定义了它的相关快捷键，使用 `<leader>gd` 可以打开 diffview 视图，使用 `<leader>gD` 则关闭。对应的命令分别是 DiffviewOpen 和 DiffviewClose。



### Lang

| 插件名称                                                     | 功能说明              |
| ------------------------------------------------------------ | --------------------- |
| **[kevinhwang91/nvim-bqf](https://github.com/kevinhwang91/nvim-bqf)** | 提供更好的 quickfix   |
| **[ray-x/go.nvim](https://github.com/ray-x/go.nvim)**        | Golang 插件           |
| **[mrcjkb/rustaceanvim](https://github.com/mrcjkb/rustaceanvim)** | Rust 插件             |
| **[Saecki/crates.nvim](https://github.com/Saecki/crates.nvim)** | 管理 `crates.io` 依赖 |
| **[iamcco/markdown-preview.nvim](https://github.com/iamcco/markdown-preview.nvim)** | 渲染 Markdown 预览    |
| **[chrisbra/csv.vim](https://github.com/chrisbra/csv.vim)**  | CSV 文件插件          |

Here is your list of plugins under **Tool** and **UI** categories, formatted with GitHub links directly embedded in the plugin names.

------

### Tool

| 插件名称                                                     | 功能说明                            |
| ------------------------------------------------------------ | ----------------------------------- |
| **[tpope/vim-fugitive](https://github.com/tpope/vim-fugitive)** | Git 操作插件，由传奇人物 tpope 编写 |
| **[Bekaboo/dropbar.nvim](https://github.com/Bekaboo/dropbar.nvim)** | 提供 winbar 面包屑导航              |
| **[nvim-tree/nvim-tree.lua](https://github.com/nvim-tree/nvim-tree.lua)** | 改进的 `netrw` 文件浏览器           |
| **[ibhagwan/smartyank.nvim](https://github.com/ibhagwan/smartyank.nvim)** | 提供 tmux/OSC52 剪贴板支持          |
| **[michaelb/sniprun](https://github.com/michaelb/sniprun)**  | 快速运行代码片段                    |
| **[akinsho/toggleterm.nvim](https://github.com/akinsho/toggleterm.nvim)** | 提供更好的终端支持                  |
| **[folke/trouble.nvim](https://github.com/folke/trouble.nvim)** | 显示代码错误                        |
| **[folke/which-key.nvim](https://github.com/folke/which-key.nvim)** | 提供键位映射提示                    |
| **[gelguy/wilder.nvim](https://github.com/gelguy/wilder.nvim)** | 命令模式补全插件                    |
| **[romgrk/fzy-lua-native](https://github.com/romgrk/fzy-lua-native)** | 为 `wilder.nvim` 提供 fzy 支持      |
| **[nvim-telescope/telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)** | 通用模糊搜索插件                    |
| **[nvim-lua/plenary.nvim](https://github.com/nvim-lua/plenary.nvim)** | `telescope.nvim` 的必需插件         |
| **[nvim-tree/nvim-web-devicons](https://github.com/nvim-tree/nvim-web-devicons)** | Nerd font 图标                      |
| **[jvgrootveld/telescope-zoxide](https://github.com/jvgrootveld/telescope-zoxide)** | 使用 `zoxide` 跳转到目录            |
| **[debugloop/telescope-undo.nvim](https://github.com/debugloop/telescope-undo.nvim)** | 模糊查找撤销历史                    |
| **[nvim-telescope/telescope-frecency.nvim](https://github.com/nvim-telescope/telescope-frecency.nvim)** | 跳转到常用或最近的文件              |
| **[nvim-telescope/telescope-live-grep-args.nvim](https://github.com/nvim-telescope/telescope-live-grep-args.nvim)** | 支持 `live_grep` 时传递参数         |
| **[nvim-telescope/telescope-fzf-native.nvim](https://github.com/nvim-telescope/telescope-fzf-native.nvim)** | 提供 `fzf` 搜索支持                 |
| **[FabianWirth/search.nvim](https://github.com/FabianWirth/search.nvim)** | 一站式 `telescope` 搜索集合面板     |
| **[ahmedkhalf/project.nvim](https://github.com/ahmedkhalf/project.nvim)** | 项目管理                            |
| **[aaronhallaert/advanced-git-search.nvim](https://github.com/aaronhallaert/advanced-git-search.nvim)** | Git 内容模糊搜索                    |
| **[tpope/vim-rhubarb](https://github.com/tpope/vim-rhubarb)** | 为 `vim-fugitive` 提供 GitHub 扩展  |
| **[sindrets/diffview.nvim](https://github.com/sindrets/diffview.nvim)** | 显示 Git diff 视图                  |
| **[mfussenegger/nvim-dap](https://github.com/mfussenegger/nvim-dap)** | Debug Adapter Protocol 客户端       |
| **[rcarriga/nvim-dap-ui](https://github.com/rcarriga/nvim-dap-ui)** | DAP 用户界面                        |
| **[nvim-neotest/nvim-nio](https://github.com/nvim-neotest/nvim-nio)** | 用于异步 IO 的库                    |
| **[jay-babu/mason-nvim-dap.nvim](https://github.com/jay-babu/mason-nvim-dap.nvim)** | 连接 `mason.nvim` 和 `nvim-dap`     |

#### Which-key

Which key 可以帮你快速查看快捷键（根据你的输入以弹窗方式展示），配置在 modules/tools/which-key.lua 中。

首先，该插件为捕捉所有的前缀键，当你按下 `<leader>` 之后，which-key 会显示当下所有与 `<leader>` 关联的按键帮助你回忆。其次，也可以通过 `which-key.register` 方法明确注册按键映射，这也是 modules/tools/which-key.lua 中做的第一件事情。

另外，使用 `:WhichKey` 可以唤出面板，然后使用 `<c-u>` 上翻，使用 `<c-d>` 下翻。



---

### UI

| 插件名称                                                     | 功能说明                     |
| ------------------------------------------------------------ | ---------------------------- |
| **[goolord/alpha-nvim](https://github.com/goolord/alpha-nvim)** | 提供更好的启动页面           |
| **[akinsho/bufferline.nvim](https://github.com/akinsho/bufferline.nvim)** | 提供标签页和缓冲区管理       |
| **[Jint-lzxy/nvim](https://github.com/Jint-lzxy/nvim)**      | `catppuccin` 主题            |
| **[j-hui/fidget.nvim](https://github.com/j-hui/fidget.nvim)** | 显示 LSP 实时状态            |
| **[lewis6991/gitsigns.nvim](https://github.com/lewis6991/gitsigns.nvim)** | 在状态列中显示 Git 状态      |
| **[lukas-reineke/indent-blankline.nvim](https://github.com/lukas-reineke/indent-blankline.nvim)** | 显示不同级别的缩进           |
| **[nvim-lualine/lualine.nvim](https://github.com/nvim-lualine/lualine.nvim)** | 简约、快速、可自定义的状态行 |
| **[zbirenbaum/neodim](https://github.com/zbirenbaum/neodim)** | 显示未使用符号的暗淡效果     |
| **[karb94/neoscroll.nvim](https://github.com/karb94/neoscroll.nvim)** | 平滑滚动                     |
| **[rcarriga/nvim-notify](https://github.com/rcarriga/nvim-notify)** | 动画通知                     |
| **[folke/paint.nvim](https://github.com/folke/paint.nvim)**  | 在缓冲区中轻松添加额外高亮   |
| **[folke/todo-comments.nvim](https://github.com/folke/todo-comments.nvim)** | 高亮显示注释中的特定关键词   |
| **[dstein64/nvim-scrollview](https://github.com/dstein64/nvim-scrollview)** | 可滚动的滚动条               |



