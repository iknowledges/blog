# Ubuntu安装nodejs

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

[Installation](https://github.com/nodejs/help/wiki/Installation)
