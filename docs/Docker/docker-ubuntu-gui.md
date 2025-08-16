# Ubuntu在Docker中运行GUI程序

1. 安装xorg

```bash
sudo apt install xorg
```

2. 设置访问X Server的权限

```bash
# non-network local connections being added to access control list
xhost +local:*
# non-network local connections being removed from access control list
xhost -local:*
# access control disabled, clients can connect from any host
xhost +
# access control enabled, only authorized clients can connect
xhost -  # -可以省略
```

3. 创建docker容器

```bash
sudo docker run --name myubt -itd --gpus all -p 6789:22 -e DISPLAY=$DISPLAY -v /tmp/.X11-unix/:/tmp/.X11-unix/ ubuntu:jammy
```
必备参数：
- -e DISPLAY=$DISPLAY 给容器设置环境变量
- -v /tmp/.X11-unix/:/tmp/.X11-unix/ 给容器挂载主机路径，用于提供X socket

4. 启动GUI程序

```bash
sudo docker exec -it myubt bash
apt update && apt install -y gedit
gedit
```

## 参考链接

[Run A GUI APPs inside a Docker Container](https://medium.com/geekculture/run-a-gui-software-inside-a-docker-container-dce61771f9)