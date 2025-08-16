# Neovim安装NvChad

## NvChad v2.5版本安装

安装命令：

```
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```

修改lua/plugins/init.lua配置插件：

```lua
return {
  ......

  {
    "neovim/nvim-lspconfig",
    lazy = false,
    config = function()
      require("nvchad.configs.lspconfig").defaults()
      require "configs.lspconfig"
    end,
  },

  {
  	"williamboman/mason.nvim",
    lazy = false,
  	opts = {
  		ensure_installed = {
            "clangd"
  		},
  	},
  },
}
```

修改lua/configs/lspconfig.lua配置语言服务器：

```lua
-- cpp
lspconfig.clangd.setup {
  on_attach = on_attach,
  on_init = on_init,
  capabilities = capabilities,
}
```

## 安装（旧版）

1. 备份一下当前配置：

```
# 备份配置
mv ~/.config/nvim ~/.config/nvim.backup
# 删除本地neovim的缓存
rm -rf ~/.local/share/nvim
```

2. 执行安装：

```
git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1
nvim # 提示是否安装示例配置，输入N
```

## 更新

```
Lazy sync
```

## 卸载

```
rm -rf ~/.config/nvim
rm -rf ~/.local/share/nvim
```

## 其他命令

### 选择主题(theming)

```
# 调出主题选择框
space+t+h
ctrl+n或ctrl+p进行上下选择
```

### 语法高亮(syntax highlight)

NvChad安装了tree-sitter插件

```
# 安装语法高亮
TSInstall cpp
# 查看已安装的语法高亮
TSInstallInfo
```

### 文件树(file tree)

NvChad安装了nvim-tree插件

```
# 打开目录
ctrl+n
# 标记文件
m
# 创建文件
a
# 复制文件
c
# 粘贴文件
p
# 删除文件
d
# 重命名文件
r
```

### 文件查看(file navigation)

```
# 查找文件
space+f+f
# 只搜索已打开的文件
space+f+b
```

### 快捷键备忘清单(cheatsheet)

```
# 打开或关闭清单
space+c+h
# 提示补全
space等待1秒
```

### 窗口跳转(window navigation)

```
# 垂直分屏
:vsp
# 水平分屏
:sp
# 切换窗口
ctrl+h/j/k/l
# 显示绝对行号
space+n
# 显示相对行号
space+r+n
```

### 标签页(tabbufline)

```
# 切换标签页
tab或shift+tab
# 关闭当前页
space+x
```

### 命令行(terminal)

```
# 水平打开命令行
space+h
# 垂直打开命令行
space+v
```

### 自定义配置(customization)

添加自定义配置的文件目录：~/.config/nvim/lua/custom

- chadrc.lua：用于覆盖NvChad的默认配置，想要更改插件时，使用该文件。
- init.lua：用于覆盖Neovim的选项和命令。

#### 配置Crystal语法高亮

1. 打开~/.config/nvim/lua/custom/目录，在该目录下新建一个plugins.lua文件，如下修改chadrc.lua文件，将插件配置放在plugins.lua文件中：

```lua
local M = {}
M.ui = {theme = 'catppuccin'}
M.plugins = 'custom.plugins'
return M
```

如下修改plugins.lua文件，设置ft为crystal指定文件类型，将crystal_auto_format设为1启用自动格式化：

```lua
local plugins = {
    {
        "vim-crystal/vim-crystal",
        ft = "crystal",
        config = function(_)
            vim.g.crystal_auto_format = 1
        end
    }
}
return plugins
```

2. 然后在vim命令模式中调用如下命令：

```
Lazy sync
```

3. 最后关闭vim重新打开crystal文件就可以看到语法高亮了。

#### 设置标准vim配置

1. 设置一行的长度不超过80个字符，修改~/.config/nvim/lua/custom/init.lua如下：

```lua
vim.opt.colorcolumn = '80'
```

### 语言服务器(language servers)

以为rust analyzer添加LSP配置为例：

1. 打开~/.config/nvim/lua/custom/chadrc.lua，修改如下：

```lua
local M = {}
M.plugins = 'custom.plugins'
return M
```

1. 打开~/.config/nvim/lua/custom/plugins.lua文件，添加一个neovim/nvim-lspconfig的条目，然后添加一个config的函数块，它包含默认的lsp配置和我们要自定义的lsp配置

```lua
local plugins = {
    ......
    {
        "neovim/nvim-lspconfig",
        config = function()
            require "plugins.configs.lspconfig"
            require "custom.configs.lspconfig"
        end,
    }
}
return plugins
```

2. 接着创建自定义lsp的配置文件custom/configs/lspconfig.lua，我们需要从主lsp配置中导入on_attach和capabilities方法，以便在配置中使用，然后还要设置文件类型，同时还要设置项目根目录规则以便服务器能识别Rust项目的根目录：

```lua
local on_attach = require("plugins.configs.lspconfig").on_attach
local capabilities = require("plugins.configs.lspconfig").capabilities

local lspconfig = require "lspconfig"

lspconfig.rust_analyzer.setup({
    on_attach = on_attach,
    capabilities = capabilities,
    filetypes = {"rust"},
    root_dir = lspconfig.util.root_pattern("Cargo.toml"),
})
```

添加其他语言的LSP配置，可以参考[nvim-lspconfig](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md)上的配置指南，或者输入命令`:help lspconfig-all`

3. 在系统上安装rust analyzer

- 方法一：使用rustup安装

```
rustup component add rust-analyzer
```

- 方法二：使用mason插件来管理LSP二进制文件

使用mason插件在自定义配置plugins.lua添加如下条目即可，然后将lsp服务器添加到ensure_installed块里面：

```lua
local plugins = {
    ......
    {
        "neovim/nvim-lspconfig",
        config = function()
            require "plugins.configs.lspconfig"
            require "custom.configs.lspconfig"
        end,
    },
    {
        "williamboman/mason.nvim",
        opts = {
            ensure_installed = {
                "rust-analyzer"
            }
        }
    }
}
return plugins
```

然后使用`:MasonInstallAll`命令下载并安装语言服务器

#### 参考资源

- [NvChad](https://nvchad.com/docs/quickstart/install)
