# IB Gateway服务器安装教程

## 一、安装VNC

### VNC服务端

1. 安装虚拟显示服务器xvfb和VNC远程服务器x11vnc：

```
# xvfb is an x11 (GUI) screen simulator
sudo apt install xvfb
# x11vnc is a remote screen simulator viewing tool
sudo apt install x11vnc
```

2. 启动虚拟屏幕：

```
/usr/bin/Xvfb :1 -ac -screen 0 1024x768x24 &
```

显示如下Xvfb进程号则表示启动成功：

```
[1] 595549
```

备注：关闭Xvfb使用如下命令：

```
sudo killall Xvfb
sudo rm -f /tmp/.X*-lock /tmp/.X11-unix/X*
```

3. 启动VNC服务：

```
/usr/bin/x11vnc -ncache 10 -ncache_cr -viewpasswd view_only_password -passwd full_access_password -display :1 -forever -shared -bg -noipv6
```

启动成功后会显示服务运行在5900端口：

```
The VNC desktop is:      10-7-94-121:0
PORT=5900
```

- -passwd full_access_password：设置有全部权限的密码，可以通过鼠标和键盘操作。
- -viewpasswd view_only_password：设置只有view权限的密码。
- -noipv6：表示不使用IPv6。
- -shared：表示允许多个viewer同时连接。
- -forever：表示VNC服务保存监听，而不是在客户端断开连接后就退出。

### VNC客户端

1. 下载[TightVNC](https://www.tightvnc.com/download.php)并安装，选择【Custom Setup】,注意这里有两个选项：【TightVNC Server】和【TightVNC Viewer】，因为本地只需要客户端功能，这里可以只选择【TightVNC Viewer】。
2. 安装完后启动TightVNC Viewer，在【Remote Host】中填入远程服务器的IP，然后点击【Connect】并输入前面设置的密码进行连接。

## 二、安装IB Gateway

1. 打开[Download IB Gateway](https://www.interactivebrokers.com/en/trading/ibgateway-latest.php)，使用下面命令进行下载安装：

```
wget https://download2.interactivebrokers.com/installers/ibgateway/stable-standalone/ibgateway-stable-standalone-linux-x64.sh
chmod a+x ibgateway-stable-standalone-linux-x64.sh
sh ibgateway-stable-standalone-linux-x64.sh -c
```

安装完成后提升你是否立即允许，选择否：

```
Run IB Gateway 10.45?
Yes [y], No [n, Enter]
n
Finishing installation ...
```

2. 设置DISPLAY环境变量和上面VNC服务一致并启动IB Gateway，如何已经连接好VNC客户端，就能在窗口看到登录界面了：

```
DISPLAY=:1 ~/Jts/ibgateway/1045/ibgateway
```

3. 卸载IB Gateway可以使用如下命令：

```
~/Jts/ibgateway/1045/uninstall -c
```

## 三、安装IBC

1. 安装解压工具：

```
sudo apt install unzip
```

2. 打开[IBC release](https://github.com/IbcAlpha/IBC/releases)，使用如下命令进行下载安装：

```
wget https://github.com/IbcAlpha/IBC/releases/download/3.24.1/IBCLinux-3.24.1.zip

sudo mkdir /opt/ibc/
sudo chown -R ubuntu:ubuntu /opt/ibc/
unzip IBCLinux-3.24.1.zip -d /opt/ibc/
cd /opt/ibc/
chmod u+x *.sh scripts/*.sh
```

3. 修改配置文件`config.ini`：

```
mkdir ~/ibc
cp /opt/ibc/config.ini ~/ibc/
vim ~/ibc/config.ini
```

将IbLoginId和IbPassword修改为你的登录名和密码：

```
# IB API Authentication Settings
# ------------------------------

# Your TWS username:

IbLoginId=edemo


# Your TWS password:

IbPassword=demouser
```

4. 修改启动脚本，注意gatewaystart.sh是IB Gateway的启动脚本，twsstart.sh是TWS的启动脚本：

```
vim /opt/ibc/gatewaystart.sh
```

修改内容如下：

```
TWS_MAJOR_VRSN=1045
IBC_INI=~/ibc/config.ini
TRADING_MODE=paper
```

- TWS_MAJOR_VRSN是客户端的主版本号，可以通过【Help】->【About Gateway】查看。
- TRADING_MODE可以选择paper（模拟盘）或live（实盘）。

5. 通过脚本启动IB Gateway：

```
DISPLAY=:1 /opt/ibc/gatewaystart.sh
```

6. 使用定时任务检测是否断连并重新登录：

```
crontab -e
```

添加如下内容，表示每个小时检查一次：

```
0 * * * * DISPLAY=:1 /opt/ibc/gatewaystart.sh
```

## 问题解决

- 启动脚本时出现如下报错：

```
xterm: command not found
```

需要安装xterm：

```
sudo apt install xterm
```

#### 参考资料

- [Setting up TWS & IBC on EC2 instance](https://dev.to/kairatorozobekov/setting-up-tws-ibc-on-ec2-instance-88b)
- [Interactive Brokers: Headless IB Gateway Installation using IBController on Ubuntu Server](https://github.com/roblav96/headless-ib-gateway-installation-ubuntu-server)
- [IBC USER GUIDE](https://github.com/IbcAlpha/IBC/blob/master/userguide.md)
