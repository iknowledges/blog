# docker部署ubuntu并远程连接ssh

#### 一、拉取容器
```
docker pull ubuntu:bionic
```
#### 二、运行容器
```
# 创建容器
docker run --name my-ubuntu -itd -p 6789:22 ubuntu:bionic
# 进入ubuntu操作界面
docker exec -it my-ubuntu /bin/bash
```
同时开启XServer访问
```
docker run --name my-ubuntu -itd -p 6789:22 -e DISPLAY=host.docker.internal:0 ubuntu:bionic
```
#### 三、下次启动和结束容器
```
docker start my-ubuntu
docker stop my-ubuntu
```
#### 四、安装ssh服务
1. 首先进入ubuntu容器，安装openssh。
```
apt update
apt install openssh-server
```
2. 设置root用户密码。
```
passwd
```
3. 修改配置文件。
```
vim /etc/ssh/sshd_config
```
> 内容如下：
```
#PermitRootLogin prohibit-password
PermitRootLogin yes
```
4. 启动ssh服务
```
service ssh start
```
5. 本机连接ssh测试
```
ssh root@127.0.0.1 -p 22
```
