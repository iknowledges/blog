# Neovim安装教程

## 一、安装Neovim

### 快速安装

```
# Ubuntu(不推荐，版本老)
sudo apt-get install neovim
# ArchLinux
sudo pacman -S neovim
```

### 手动安装

1. 下载`nvim-linux64.tar.gz`

```
wget https://github.com/neovim/neovim/releases/download/stable/nvim-linux64.tar.gz
```

2. 解压压缩包

```bash
tar xzvf nvim-linux64.tar.gz
```

3. 修改`/etc/profile`配置环境变量

```
export PATH=/path/to/nvim-linux64/bin:$PATH
alias vim=nvim
```

## 二、安装字体Nerd Font（本地终端选择安装）

1. 下载字体，如：[JetbrainsMono Nerd Font](https://www.nerdfonts.com/font-downloads)

```
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.1.1/JetBrainsMono.zip
```

2. 解压压缩包并拷贝到系统字体目录

```
sudo unzip JetBrainsMono.zip -d /usr/local/share/fonts/JetBrainsMono/
# 更新字体
sudo fc-cache -fv
```

- 报错：-bash: fc-cache: command not found

```
# CentOS
yum install fontconfig
# Ubuntu
apt install fontconfig
```

3. 打开[GNOME terminal]->[首选项]->[配置文件]，自定义字体选择JetbrainsMono Nerd Font

4. 其他设置

- 安装xclip，使Neovim与系统剪切板互通

```
sudo apt install xclip
```

#### 参考资源

- [neovim](https://github.com/neovim/neovim/blob/master/INSTALL.md)
