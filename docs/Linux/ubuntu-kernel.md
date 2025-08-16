# Ubuntu内核管理

#### 卸载不需要要的内核

1. ubuntu开机按Shift键进入Advance模式，选择需要的内核版本进入桌面。

2. 执行下面命令：

```
# 查看已安装的内核
sudo dpkg --get-selections | grep linux
# 卸载旧的内核
sudo apt purge linux-headers-6.2.0-26-generic
sudo apt purge linux-image-6.2.0-26-generic
sudo apt autoremove
# 更新启动引导
sudo update-grub
```

3. 重启电脑，查看内核版本

```
uname -r
```