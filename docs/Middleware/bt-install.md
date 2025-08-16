# 宝塔面板使用教程

1. 安装脚本

```
# CentOS
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh
# Ubuntu
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh
```

2. 卸载脚本

```
wget http://download.bt.cn/install/bt-uninstall.sh
sudo sh bt-uninstall.sh
```

3. 常用命令

```
# 卸载
/etc/init.d/bt stop && chkconfig --del bt && rm -f /etc/init.d/bt && rm -rf /www/server/panel
# 命令行工具箱
bt
```

#### 参考资源

- [安装方法](https://www.bt.cn/linux.html)
- [宝塔linux面板命令大全](https://www.bt.cn/new/btcode.html)