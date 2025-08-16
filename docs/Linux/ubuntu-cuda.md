# Ubuntu安装cuda

下载地址：https://developer.nvidia.com/cuda-toolkit-archive

安装过程需要用ssh连接到主机

1. 关闭X服务

```
sudo /etc/init.d/gdm3 stop
sudo /etc/init.d/gdm3 status
```

2. 禁用Nouveau驱动

```
sudo vim /etc/modprobe.d/blacklist-nouveau.conf
```

写入如下内容：

```
blacklist nouveau
options nouveau modeset=0
```

更新使配置生效，然后重启：

```
sudo update-initramfs -u
sudo reboot
```

检查是否禁用成功：

```
lspci | grep nouveau
```

3. 安装cuda

```
sudo sh cuda_12.1.0_530.30.02_linux.run
```