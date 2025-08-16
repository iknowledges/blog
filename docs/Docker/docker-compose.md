# docker compose使用教程

1. 新建Dockerfile

```docker
FROM ubuntu:jammy

RUN sed -i s/archive.ubuntu.com/mirrors.aliyun.com/g /etc/apt/sources.list \
    && sed -i s/security.ubuntu.com/mirrors.aliyun.com/g /etc/apt/sources.list \
    && apt update && apt upgrade -y \
    && apt install -y sudo
RUN useradd -m admin \
	&& echo "admin:admin" | chpasswd \
	&& adduser admin sudo \
	&& echo "admin ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers
USER admin
WORKDIR /home/admin
RUN sudo apt install -y vim net-tools wget curl build-essential gdb
```

启动容器后安装其他包
```bash
sudo apt install -y openssh-server git cmake nodejs npm python3 python3-pip
```

2. 编译镜像

```bash
# 编译镜像
docker build -t devubt:jammy .
# 删除镜像
docker rmi devubt:jammy
```

3. 新建docker-compose.yml

```yaml
version: '3.8'
services:
  myubt:
    image: devubt:jammy
    ports:
      - "3000:3000"
      - "6789:22"
    environment:
      - DISPLAY=host.docker.internal:0.0
    volumes:
      - /tmp/.X11-unix/:/tmp/.X11-unix/
      - my_work:/home/admin/workspace
    tty: true
volumes:
  my_work:
```

4. 容器操作命令
```bash
# 创建容器
docker-compose up -d
# 停止并删除容器
docker-compose down
# 启动容器
docker-compose start
# 停止容器
docker-compose stop
# 进入容器
docker-compose exec myubt bash
# 指定用户和工作目录进入容器
docker-compose exec -u admin -w /home/admin myubt bash
# 列出所有挂载
docker volume ls
# 查看指定挂载信息
docker volume inspect ubuntu_my_work
# 移除指定挂载
docker volume rm ubuntu_my_work
```

> Windows中的挂载路径

通过inspect命令可以看到"Mountpoint": "/var/lib/docker/volumes/ubuntu_my_work/_data"，可以在win10的文件资源管理器输入如下路径：
```
\\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes
```

### 参考资料

[Docker Desktop for Windows（WSL 2 方式）数据卷位置和访问](https://blog.csdn.net/u013568040/article/details/119899675)
