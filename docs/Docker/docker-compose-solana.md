# 使用docker compose安装solana开发环境

1. 编写Dockerfile：

```
FROM ubuntu:noble

RUN apt update
RUN apt install -y sudo
RUN useradd -m -s /bin/bash -G sudo admin \
	&& echo "admin:admin" | chpasswd \
	&& echo "admin ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers
USER admin
WORKDIR /home/admin
RUN sudo apt-get install -y vim net-tools wget curl git openssh-server
# 安装Solana
RUN sudo apt-get install -y \
    build-essential \
    pkg-config \
    libudev-dev llvm libclang-dev \
    protobuf-compiler libssl-dev
RUN curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
```

2. 编写docker-compose.yml

```yaml
version: '3.8'
services:
  ubuntu24:
    # 使用当前目录下的Dockerfile构建镜像
    build: .
    container_name: ubuntu24-dev
    ports:
      - "22:22"
      - "8899:8899"
      - "8900:8900"
    volumes:
      - ../DockerProject:/home/admin/workspace
    working_dir: /home/admin/workspace
    # 保持标准输入打开并分配伪终端（tty）
    # 使容器可以运行交互式命令行程序
    tty: true
```

3. 执行命令

```
# 构建镜像
docker-compose build
# 启动指定容器，注意这个命令端口映射不会生效
docker-compose run --rm ubuntu24
# 初始化并启动所有容器
docker-compose up -d
# 停止并删除容器
docker-compose down
# 启动容器
docker-compose start
# 停止容器
docker-compose stop
# 进入容器
docker exec -it ubuntu24 bash
```