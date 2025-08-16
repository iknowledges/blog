# Mac在Docker中运行GUI程序

### 一、安装X11服务XQuartz

1. 下载[XQuartz](https://www.xquartz.org/)安装文件，或者也可以用Homebrew安装，命令如下：
```bash
# 安装xquartz
brew install --cask xquartz
# 查看安装的包
brew list
# 卸载xquartz
brew uninstall xquartz
```
2. 启动XQuartz，并设置“允许从网络客户端连接”：

![XQuartz](https://gitlab.com/iknowledge/BlogImage/-/raw/main/XQuartz/xquartz.jpeg)

3. 重新启动XQuartz

### 二、运行Docker容器

1. 输入第一条命令使所有用户都能访问Xserver
```bash
# 开启访问权限
xhost +
# 关闭访问权限
xhost -
```

2. 在docker中创建一个ubuntu，DISPLAY是运行X11需要的环境变量，host.docker.internal是docker访问宿主机的IP地址
```bash
docker run --name firefox -e DISPLAY=host.docker.internal:0 -itd lazybones3/ubuntu:bionic
```
`-e DISPLAY=host.docker.internal:0`创建时可以不设置，等启动容器后在交互界面手动输入`export DISPLAY=host.docker.internal:0`

3. 进入docker交互界面
```bash
docker exec -it firefox bash
```

4. 安装firefox
```bash
apt update && apt install -y firefox
```
5. 在命令行运行`firefox`就能弹出火狐浏览器窗口

#### 参考资料

[Run Linux/X11 apps in Docker and display on a Mac OS X desktop](https://techsparx.com/software-development/docker/display-x11-apps.html)
[docker_x11_macOS](https://gist.github.com/paul-krohn/e45f96181b1cf5e536325d1bdee6c949)
[x11_docker_mac](https://gist.github.com/cschiewek/246a244ba23da8b9f0e7b11a68bf3285)
