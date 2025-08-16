# Docker离线部署

## 离线安装docker

1. 使用`uname -a`查看内核版本，并下载相应[docker二进制文件](https://download.docker.com/linux/static/stable/)

2. 解压安装包
```bash
tar xzvf /path/to/<FILE>.tar.gz
```

3. 将二进制文件拷贝到可执行路径
```bash
sudo cp docker/* /usr/bin/
```

4. 后台启动docker
```bash
sudo dockerd &
```

## 离线安装镜像

1. 导出java镜像
```bash
docker save java:8 -o java.tar
```

2. 离线导入镜像
```bash
docker load -i java.tar
```

#### 参考资料

[Install Docker Engine from binaries](https://docs.docker.com/engine/install/binaries/)
