# Windows虚拟机安装virtio

1. 下载[virtio-win-0.1.229.iso](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/)

2. kvm创建windows虚拟机，并修改启动配置，然后开始安装：

- 添加硬件->存储->CDROM设备，挂载上一步下载的iso文件
- 引导选项->勾选CDROM
- 磁盘->磁盘总线->VirtIO
- NIC->设备型号->virtio
- 显卡->型号->Virtio

3. 按住键盘的 Shift + F10 打开终端界面，输入 regedit 打开注册表：

在 HKEY_LOCAL_MACHINE\SYSTEM\Setup 右键新建一个项，命名为LabConfig，在该项右键新建三个DWORD(32位)值，分别命名为 BypassTPMCheck 、BypassRAMCheck 和 BypassSecureBootCheck，并将这三个值都设置为1

4. 选择自定义安装，在Windows安装界面没有找到磁盘，这时点击【加载驱动程序】->【浏览】，选择挂载virtio-win-0.1.229.iso的驱动器，如：E:\amd64\w11\viostor.inf，安装磁盘驱动。

5. 跳过联网安装：Shift + F10 打开终端，输入OOBE\BypassNRO.cmd，等待重启后就可以跳过联网。

6. 设备管理器：右键带感叹号的设备，点击【更新驱动程序】->【浏览我的电脑以查找驱动程序】，选择挂载virtio-win-0.1.229.iso的驱动器，点击下一步，按照提示安装驱动。

7. 下载[spice-guest-tools](https://www.spice-space.org/download.html)，安装剪切板共享功能。

#### 参考资料

- [在KVM上的Windows中安装Virtio驱动程序](https://blog.csdn.net/allway2/article/details/103125982)
- [Windows安装VirtIO驱动的两种方法](https://www.cnblogs.com/hahaha111122222/p/15549004.html)
