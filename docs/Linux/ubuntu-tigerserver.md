# Ubuntu安装tigerserver

## 服务端

1. 安装TigerVNC服务

```bash
# install TigerVNC
sudo apt install tigervnc-standalone-server
# 然后初始化
vncserver
```
设置密码，Would you like to enter a view-only password (y/n)?选择n

2. 配置GNOME桌面环境

```bash
sudo vim ~/.vnc/xstartup
```

写入内容：
```bash
#!/bin/sh
# Start up the standard system desktop
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/gnome-session
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &
```

```bash
# 关闭刚刚初始化时创建的服务
vncserver -kill :1
# 添加可执行权限
sudo chmod +x ~/.vnc/xstartup
# 启动服务
vncserver -localhost no :1
```

3. 其他操作

```bash
# 关闭
vncserver -kill :1
# 启动
vncserver -localhost no :1
# 查看
vncserver -list
# 修改密码
vncpasswd
```

4. 卸载服务

```bash
sudo apt remove tigervnc-standalone-server
sudo apt autoremove
sudo rm -rf ~/.vnc/
```

#### 安装XFCE桌面环境

```bash
sudo apt install xfce4 xfce4-goodies
```

~/.vnc/xstartup文件的内容：
```
#!/bin/sh
# Start up the standard system desktop
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/startxfce4
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &
```

## 客户端

1. 下载[VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)
2. 输入VNC Server：192.168.3.17:1，name随便填，然后点击OK进行连接。

## 问题排查

执行vncserver报错：Failed to execute child process ?dbus-launch? (No such file or directory)
解决方法：
```bash
sudo apt install dbus-x11
sudo service dbus start
```

## 参考链接
[How to Install & Configure VNC Server on Ubuntu 22.04|20.04](https://bytexd.com/how-to-install-configure-vnc-server-on-ubuntu/)
