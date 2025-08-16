# Ubuntu安装xrdp

## 安装xrdp服务端

### 方式一：apt安装

参考：[How to Install xRDP (Remote Desktop) on Ubuntu 22.04|20.04](https://bytexd.com/xrdp-ubuntu/)

```bash
sudo apt -y install xrdp
# 查看运行状态
sudo systemctl status xrdp
# 将xrdp添加到ssl-cert用户组(可选)
# 注：Ubuntu22执行这条命令会出现黑屏
sudo adduser xrdp ssl-cert
# 配置防火墙（可选）
sudo ufw allow 3389
# 重启服务
sudo systemctl restart xrdp
```

安装xfce桌面环境
```bash
sudo apt install xfce4
```

### 方式二：脚本安装

参考：[xRDP – Easy install xRDP on Ubuntu 20.04,22.04,23.XX (Script Version 1.4.8)](https://c-nergy.be/blog/?p=19228)

- 安装

```
# 下载脚本
wget https://www.c-nergy.be/downloads/xRDP/xrdp-installer-1.4.8.zip
# 解压
unzip xrdp-installer-1.4.8.zip
chmod +x ~/Downloads/xrdp-installer-1.4.8.sh
# -s代表安装声音模块，但声音模块存在卡顿的问题(choppy sound)
./xrdp-installer-1.4.8.sh -s
# 完成后记得重启
```

- 卸载

```
./xrdp-installer-1.4.8.sh -r 
```

## 客户端

服务端使用passwd设置用户名和密码，然后客户端Microsoft Remote Desktop进行登录

PC name: 192.168.3.17:3389

## 其他操作
```bash
sudo systemctl start xrdp
sudo systemctl stop xrdp
# 启用开机启动
sudo systemctl enable xrdp xrdp-sesman
# 禁用开机启动
sudo systemctl disable xrdp xrdp-sesman
# 查看运行依赖项
systemctl list-dependencies xrdp
# 将xrdp从ssl-cert用户组删除
sudo deluser xrdp ssl-cert
```

## 问题

- Ubuntu22连接后黑屏

```bash
# 查看登陆的用户名
who
# 注销用户
sudo pkill -9 -u roy
```

## 参考链接

- [Logging out other users from the command line](https://askubuntu.com/questions/12180/logging-out-other-users-from-the-command-line)
- [XRDP](https://c-nergy.be/blog/?cat=79)
