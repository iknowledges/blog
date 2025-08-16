# vim-plug教程

Link: https://github.com/junegunn/vim-plug/wiki/tutorial
Type: Other

# 安装

下载plug.vim文件放到~/.vim/autoload/plug.vim目录，也可以用以下命令安装：

```bash
# Vim (~/.vim/autoload)
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# Neovim (~/.local/share/nvim/site/autoload)
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

# 安装插件

在~/.vimrc文件写入下面内容：

```bash
" Plugins will be downloaded under the specified directory.
call plug#begin(has('nvim') ? stdpath('data') . '/plugged' : '~/.vim/plugged')

" Declare the list of plugins.
Plug 'tpope/vim-sensible'
Plug 'junegunn/seoul256.vim'

" List ends here. Plugins become visible to Vim after this call.
call plug#end()
```

重启Vim，运行 `:PlugInstall`指令安装插件。

# 卸载插件

1. 删除~/.vimrc文件中插件对应的 `Plug` 命令
2. 重启Vim并运行 `:PlugClean`指令。