### core.pack

这个文件基于 Lazy 做插件的安装、配置工作。从 load_lazy() 函数的最后一条执行语句 require("lazy").setup(self.modules, lazy_settings) 可以看出，它从 modules 文件夹中拉去插件，并使用 lazy_settings 这个对象做设置。

具体加载 plugins 的逻辑在 load_plugins() 函数中，get_plugins_list 拿到 modules/plugins 和 user/plugins 下的所有以 .lua 结尾的文件，认为这些都是 plugins 相关的文件。

这些文件中，都以  name : config 的形式存放每个插件以及相关信息，将这放入 self.modules 中，然后 require("lazy").setup(self.modules, lazy_settings)  就可以进行配置了。

配置 leader 键，在 lua/core/init.lua 文件的 leader_map 函数中。

默认为 vim.g.mapleader = " "，表示空格键为 leader，若删除这一行，则恢复至 nvim 的默认键 `\` 。当然也可以添加这两行，将空格替换为空字符串：

```lua
vim.api.nvim_set_keymap("n", " ", "", { noremap = true })
vim.api.nvim_set_keymap("x", " ", "", { noremap = true })
```

