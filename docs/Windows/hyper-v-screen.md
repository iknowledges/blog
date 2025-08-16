# Hyper-V中运行Ubuntu全屏黑边

1. 打开配置文件

```bash
cd /etc/default
sudo gedit grub
```

2. 将`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"`修改为下面内容：

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=hyperv_fb:1920x1080"
```

3. 更新配置，然后重启

```bash
sudo update-grub
```