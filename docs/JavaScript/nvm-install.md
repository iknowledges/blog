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
# 安装最新版node
nvm install node
# 使用最新版node
nvm use node
# 安装LTS版node
nvm install --lts
# 使用LTS版node
nvm use --lts
```

5. 验证安装是否成功，检查Node版本：

```
node --version
```

## Windows中安装

### Version 1.x.x

1. 下载[nvm-noinstall.zip](https://github.com/coreybutler/nvm-windows/releases)并解压。
2. 在解压目录创建[settings.txt](https://github.com/coreybutler/nvm/tree/master/examples)，内容如下：

```
root: D:\Software\nvm\nvm-noinstall
path: D:\Software\nvm\nodejs
arch: 64
proxy: none
```

- root: 表示安装路径，对应环境变量NVM_HOME
- path: 表示符号链接目录，对应环境变量NVM_SYMLINK
- proxy: 表示网络代理
- arch: 表示32位或64位系统

3. 配置系统环境变量，并将`%NVM_HOME%;%NVM_SYMLINK%`添加到`PATH`

```
NVM_HOME: D:\Software\nvm\nvm-noinstall
NVM_SYMLINK: D:\Software\nvm\nodejs
```

4. 测试nvm是否安装成功：

```
nvm -v
```

### Version 2

1. 下载[nvm-2.0.1-x64-setup.exe](https://github.com/nvm-windows/nvm/releases)并运行安装。

- NVM blocks npm in that case (NVM4305) unless you harden permissions or use a private path.
    - Harden permissions for this path (recommended; Windows may ask for approval)
- Shim mode offers an enhanced modern workflow. Link mode offers guaranteed minimum latency with no extra features. This can be changed at any time by running "nvm use <mode>".
    - Shim
- These settings can be changed at any time using the "nvm config" command.
    - Auto-detect Node.js version, e.g. .nvmrc, .node-version, package.json (shim mode)
    - Require valid TLS/SSL certificates from download mirrors

2. 修改配置

```
# 设置Node.js安装根目录
nvm config set root="D:\Software\nvm\installs"
# 淘宝镜像站
nvm config set node_mirror=https://npmmirror.com/mirrors/node
nvm config set npm_mirror=https://registry.npmmirror.com
# 查看配置
nvm config list
```

3. 测试nvm是否安装成功：

```
nvm -v
```

### 常用命令

```
# 安装最新lts版
nvm install lts
# 查看24的版本信息
nvm list releases 24 --json
# 安装指定版本
nvm install 24.15.0
# 查看已安装的nodejs
nvm ls
# 使用已安装的nodejs
nvm use 24.15.0
# 卸载并清除缓存
nvm uninstall 24.15.0 --purge
```

#### 参考资料

- [Node Version Manager](https://github.com/nvm-sh/nvm)
- [nvm-windows v1 - manual installation](https://github.com/nvm-windows/nvm/wiki#manual-installation)
- [nvm-windows v2 - installation](https://docs.nvm-windows.com/install/installers)
