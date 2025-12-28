# Ubuntu安装nodejs

## 使用 Node Version Manager (nvm) 安装

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

## 通过二进制包安装

1. 下载二进制安装包并解压：https://nodejs.org/en/download

```
tar -xJvf node-v18.17.0-linux-x64.tar.xz
```

2. 在`~/.bashrc`写入如下内容：

```
# Nodejs
VERSION=v18.17.0
DISTRO=linux-x64
export PATH=/home/sheeran/Desktop/Software/node-$VERSION-$DISTRO/bin:$PATH
```

3. 激活配置

```
source ~/.bashrc
node -v
```

4. 配置镜像源

```
// 查看镜像源
npm config get registry

// 设置镜像源
npm config set registry http://registry.npm.taobao.org/
npm config set registry https://registry.npmjs.org/
```

#### 参考资料

- [Installation](https://github.com/nodejs/help/wiki/Installation)
- [Node Version Manager](https://github.com/nvm-sh/nvm)
