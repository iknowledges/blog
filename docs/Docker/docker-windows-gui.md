# Windows在Docker中运行GUI程序

### 一、安装Windows的X11服务VcXsrv

1. 下载[VcXsrv](https://sourceforge.net/projects/vcxsrv/)安装文件
2. 启动VcXsrv服务，并按照下图设置

Display number可以改为0，-1是自动选择
![01](https://gitlab.com/iknowledge/BlogImage/-/raw/main/VcXsrv/vcxsrv01.png)

![02](https://gitlab.com/iknowledge/BlogImage/-/raw/main/VcXsrv/vcxsrv02.png)

注意勾选Diable access control
![03](https://gitlab.com/iknowledge/BlogImage/-/raw/main/VcXsrv/vcxsrv03.png)

![04](https://gitlab.com/iknowledge/BlogImage/-/raw/main/VcXsrv/vcxsrv04.png)

### 二、运行Docker容器

1. 在docker中创建一个ubuntu，DISPLAY是运行X11需要的环境变量，host.docker.internal是docker访问宿主机的IP地址
```bash
docker run --name firefox -itd -e DISPLAY=host.docker.internal:0.0 ubuntu:bionic
```
2. 进入docker交互界面
```bash
docker exec -it firefox bash
```
3. 安装firefox
```bash
apt update && apt install -y firefox
```
4. 在命令行运行`firefox`就能弹出火狐浏览器窗口

### 四、参考
[在Docker for Windows中运行GUI程序](https://www.cnblogs.com/larva-zhh/p/10531824.html)

[Run GUI app in linux docker container on windows host](https://dev.to/darksmile92/run-gui-app-in-linux-docker-container-on-windows-host-4kde)
