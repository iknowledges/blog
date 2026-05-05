# Node Version Manager (nvm) 安装教程

## Ubuntu中安装

1. 运行安装命令：

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
```

如果下载太慢可以先下载好install.sh脚本，然后修改NVM_SOURCE_URL：

```
# NVM_SOURCE_URL="https://github.com/${NVM_GITHUB_REPO}.git"
NVM_SOURCE_URL="https://gitee.com/mirrors/nvm.git"
```

最后执行安装脚本：

```
bash install.sh
```

2. 添加环境变量：

```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

3. 测试nvm是否安装成功：

```
source ~/.bash_profile
nvm -v
```

4. 安装Node：

```
nvm install node
```

5. 验证安装是否成功，检查Node版本：

```
node --version
```

## Windows中安装

1. 下载[nvm-noinstall.zip](https://github.com/coreybutler/nvm-windows/releases)并解压。
2. 在解压目录创建[settings.txt](https://github.com/coreybutler/nvm/tree/master/examples)，内容如下：

```
root: D:\Software\NVM\nvm-noinstall
path: D:\Software\NVM\nodejs
arch: 64
proxy: none
```

- root: 表示安装路径，对应环境变量NVM_HOME
- path: 表示符号链接目录，对应环境变量NVM_SYMLINK
- proxy: 表示网络代理
- arch: 表示32位或64位系统

3. 配置系统环境变量，并将`%NVM_HOME%;%NVM_SYMLINK%`添加到`PATH`

```
NVM_HOME: D:\Software\NVM\nvm-noinstall
NVM_SYMLINK: D:\Software\NVM\nodejs
```

4. 测试nvm是否安装成功：

```
nvm -v
```

5. 安装nodejs:

```
# 安装长期支持版
nvm install lts
# 查看已安装的nodejs
nvm ls
# 使用已安装的nodejs
nvm use 24.15.0
```

#### 参考资料

- [Node Version Manager](https://github.com/nvm-sh/nvm)
- [nvm-windows](https://github.com/coreybutler/nvm-windows/wiki#manual-installation)
