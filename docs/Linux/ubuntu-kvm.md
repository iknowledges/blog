# Ubuntu安装KVM

## 宿主机

```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
sudo systemctl is-active libvirtd
```

- qemu-kvm - 为KVM管理程序提供硬件模拟的软件程序
- libvirt-daemon-system - 将libvirt守护程序作为系统服务运行的配置文件
- libvirt-clients - 用来管理虚拟化平台的软件
- bridge-utils - 用来配置网络桥接的命令行工具
- virtinst - 用来创建虚拟机的命令行工具
- virt-manager - 提供一个易用的图形界面，并且通过libvirt支持用于管理虚拟机的命令行工具

## 虚拟机

### Ubuntu

```
sudo apt install spice-vdagent
sudo systemctl enable spice-vdagent
```

### Windows 11

1. 磁盘总线用默认SATA，不能选VirtIO，否则安装时找不到磁盘。

2. 按住键盘的 Shift + F10 打开终端界面，输入 regedit 打开注册表：

- 在 HKEY_LOCAL_MACHINE\SYSTEM\Setup 右键新建一个项，命名为LabConfig，在该项右键新建三个DWORD(32位)值，分别命名为 BypassTPMCheck 、BypassRAMCheck 和 BypassSecureBootCheck，并将这三个值都设置为1

3. 跳过联网安装：Shift + F10 打开终端，输入OOBE\BypassNRO.cmd，等待重启后就可以跳过联网。

4. 下载[virtio-win-gt-x64](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/?C=M;O=D)，安装virtio驱动。

5. 下载[spice-guest-tools](https://www.spice-space.org/download.html)，安装剪切板共享功能。

## 问题解决

- 删除虚拟机出错 'ubuntu20'：Requested operation is not valid: cannot undefine domain with nvram

```
virsh list --all
virsh undefine ubuntu20 --nvram
```

- KVM创建快照失败 Operation not supported: internal snapshots of a VM with pflash based firmware are not supported

(1)进入宿主机，切换为root用户，编辑虚拟机配置文件：

```
virsh list --all
virsh edit ubuntu20
```

(2)搜索关键字`loader`，定位到需要修改字段的行

(3)将字段`type=pflash`修改为`type=rom`，然后保存退出，如：

```
<loader readonly='yes' type='rom'>/usr/share/OVMF/OVMF_CODE.ms.fd</loade>
```

(4)关闭并重新启动虚拟机

```
virsh shutdown ubuntu20
virsh start ubuntu20
# 查看配置是否修改成功
virsh dumpxml ubuntu20 | grep loader
```

(5)此方法用Windows虚拟机时导致虚拟机无法启动，目前没有解决。

#### 参考资料

- [KVM : GPU Passthrough](https://www.server-world.info/en/note?os=Debian_12&p=kvm&f=13)
- [PCI passthrough via OVMF](https://wiki.archlinuxcn.org/wiki/PCI_passthrough_via_OVMF)
- [单显卡直通教程](https://liucreator.gitlab.io/zh/posts/0x0b-single-gpu-passthrough/main/)
- [KVM 虚拟机 和 单显卡直通](https://www.bilibili.com/read/cv12760387/)
- [共享剪切版，文件拖拽进虚拟机](https://blog.csdn.net/qq_33831360/article/details/123700719)
- [kvm 安装Windows11的时候提示这台电脑无法运行 Windows 11解决办法](https://blog.51cto.com/u_13849441/5684830)
- [Win11安装 (Win11 22H2) 不联网 跳过联网 激活 终极教程](https://blog.csdn.net/chen20170325/article/details/127615233)
- [Windows VirtIO Drivers](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers#Windows_OS_Support)
- [KVM创建快照失败](https://www.cnblogs.com/anderly/p/14977989.html)
