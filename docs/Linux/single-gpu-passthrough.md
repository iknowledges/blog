# KVM单显卡直通教程

## 1 配置一个简单的KVM（非直通）

### 1.1 启用IOMMU

1. 修改配置文件

```
sudo nano /etc/default/grub
```

在`GRUB_CMDLINE_LINUX_DEFAULT`后面添加：

- AMD处理器：`amd_iommu=on`
- 英特尔处理器：`intel_iommu=on`
- 可视情况添加（修复或导致黑屏）：`iommu=pt`

使用以下命令生成配置后，重启电脑：

```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

2. 检查IOMMU是否已启用

```
sudo dmesg | grep -e DMAR -e IOMMU
# 找找是否有amd_IOMMU:Detected或Intel-IOMMU: enabled类似的信息
```

3. 运行下面的脚本，看看显卡是不是在一个IOMMU组里面，一个IOMMU组是将物理设备直通给虚拟机的最小单位：

iommu.sh
```
#!/bin/bash
shopt -s nullglob
for g in $(find /sys/kernel/iommu_groups/* -maxdepth 0 -type d | sort -V); do
    echo "IOMMU Group ${g##*/}:"
    for d in $g/devices/*; do
        echo -e "\t$(lspci -nns ${d##*/})"
    done;
done;
```

复制显卡信息，先保存下来。

```
IOMMU Group 13:
	01:00.0 VGA compatible controller [0300]: NVIDIA Corporation Device [10de:2504] (rev a1)
	01:00.1 Audio device [0403]: NVIDIA Corporation Device [10de:228e] (rev a1)
```

### 1.2 添加VFIO到内核模块

1. 修改配置

```
sudo nano /etc/modprobe.d/vfio.conf
```

添加如下内容：

```
options vfio-pci ids=1002:67ff,1002:aae0
options vfio-pci disable_idle_d3=1
options vfio-pci disable_vga=1
```

ids=后面是刚才查看的显卡的id，每个值之间用逗号隔开。

2. 安装依赖包

- Archlinux

```
sudo pacman -S qemu libvirt virt-manager bridge-utils edk2-ovmf ebtables iptables dnsmasq
```

- Ubuntu

```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virtinst virt-manager ovmf qemu
# 网络相关
sudo apt install ebtables iptables dnsmasq
```

  - qemu-kvm: 提供硬件底层虚拟化
  - libvirt-daemon-system: 为libvirt作为系统服务的守护程序运行
  - libvirt-clients: 为不同的虚拟机提供长期稳定的C API
  - bridge-utils: 提供网络桥接功能
  - virtinst: 为libvirt创建虚拟机提供一系列的命令行工作
  - virt-manager: KVM虚拟机管理图形界面，如果服务器没有安装图形化界面，没有必要安装它
  - ovmf: 等同于edk2-ovmf

3. 启动libvirt服务

```
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
```

4. 打开Virt-Manager安装一个没有直通的虚拟机

## 2 Libvirt 钩子

1. 在libirt里面创建一个hooks文件夹

```
sudo mkdir /etc/libvirt/hooks
cd /etc/libvirt/hooks
```

把下面三个文件放到hooks文件夹里面：

> **vfio-startup.sh**

```
#!/bin/bash
# Helpful to read output when debugging
set -x

sleep 5

# Stop display manager
systemctl stop <your-display-manager>

# Unbind VTconsoles
echo 0 > /sys/class/vtconsole/vtcon0/bind
echo 0 > /sys/class/vtconsole/vtcon1/bind

# Unbind EFI-Framebuffer
echo efi-framebuffer.0 > /sys/bus/platform/drivers/efi-framebuffer/unbind

# Avoid a Race condition
sleep 10

modprobe -r nvidia_uvm
modprobe -r nvidia_drm
modprobe -r nvidia_modeset
modprobe -r nvidia
modprobe -r drm_kms_helper
modprobe -r i2c_nvidia_gpu
modprobe -r drm

# Unbind the GPU from display driver
virsh nodedev-detach <pci_0000_AB_CD_E>

# Load VFIO Kernel Module
modprobe vfio_pci
modprobe vfio_iommu_type1
modprobe vfio
modprobe vfio_virqfd
```

> **vfio-teardown.sh**

```
#!/bin/bash
set -x

sleep 10

modprobe -r vfio_pci
modprobe -r vfio_iommu_type1
modprobe -r vfio
modprobe -r vfio_virqfd

# Re-Bind GPU to Nvidia Driver
virsh nodedev-reattach <pci_0000_AB_CD_E>

# Rebind VT consoles
echo 1 > /sys/class/vtconsole/vtcon0/bind
echo 1 > /sys/class/vtconsole/vtcon1/bind

echo efi-framebuffer.0 > /sys/bus/platform/drivers/efi-framebuffer/bind

# Reload nvidia modules
modprobe nvidia_uvm
modprobe nvidia_drm
modprobe nvidia_modeset
modprobe nvidia
modprobe drm_kms_helper
modprobe i2c_nvidia_gpu
modprobe drm

sleep 5

# Restart Display Manager
systemctl start <your-display-manager>
```

- 修改<pci_0000_AB_CD_E>：打开前面保存的显卡信息，把前面的0a:00.0和0a:00.1看成AB:CD.E，把pci_0000_AB_CD_E按字母替换，你的显卡有几个部分，就需要几行。
- 修改<your-display-manager>：改成你自己的显示管理器的服务，比如sddm，可以尝试用下面命令查看显示管理器。

```
file /etc/systemd/system/display-manager.service
```

> **qemu**

```
#!/bin/bash

OBJECT="$1"
OPERATION="$2"

if [[ $OBJECT == "win10" ]]; then
	case "$OPERATION" in
        	"prepare")
                systemctl start libvirt-nosleep@"$OBJECT"  2>&1 | tee -a /var/log/libvirt/custom_hooks.log
                /bin/vfio-startup.sh 2>&1 | tee -a /var/log/libvirt/custom_hooks.log
                ;;

            "release")
                systemctl stop libvirt-nosleep@"$OBJECT"  2>&1 | tee -a /var/log/libvirt/custom_hooks.log  
                /bin/vfio-teardown.sh 2>&1 | tee -a /var/log/libvirt/custom_hooks.log
                ;;
	esac
fi
```

2. 让文件可以执行

配置执行权限：

```
sudo chmod +x /etc/libvirt/hooks/vfio-startup.sh
sudo chmod +x /etc/libvirt/hooks/vfio-teardown.sh
sudo chmod +x /etc/libvirt/hooks/qemu
ls -la /etc/libvirt/hooks
```

创建软链接：

```
sudo ln -s /etc/libvirt/hooks/vfio-startup.sh /bin/vfio-startup.sh
sudo ln -s /etc/libvirt/hooks/vfio-teardown.sh /bin/vfio-teardown.sh
```

3. 防止Libvirt在运行时休眠

```
sudo nano /etc/systemd/system/libvirt-nosleep@.service
```

添加内容如下：

```
[Unit]
Description=Preventing sleep while libvirt domain "%i" is running

[Service]
Type=simple
ExecStart=/usr/bin/systemd-inhibit --what=sleep --why="Libvirt domain \"%i\" is running" --who=%U --mode=block sleep infinity
```

更改权限：

```
sudo chmod 644 -R /etc/systemd/system/libvirt-nosleep@.service
```

变成系统文件：

```
sudo chown root:root /etc/systemd/system/libvirt-nosleep@.service
```

## 3 QEMU和Libvirt的配置

1. 编辑libvirt.conf

```
sudo nano /etc/libvirt/libvirtd.conf
```

找到这两行，把前面的#删掉：

```
unix_sock_group = "libvirt"
unix_sock_rw_perms = "0770"
```

可以选择把这两行添加到文件的最后面（日志文件）:

```
log_filters="1:qemu"
log_outputs="1:file:/var/log/libvirt/libvirtd.log"
```

2. 编辑qemu.conf

```
sudo nano /etc/libvirt/qemu.conf
```

- `#user = "root"`改成`user = "yourusername"`
- `#group = "root"`改成`group = "libvirt"`

添加到libvirt用户组：

```
sudo usermod -aG libvirt yourusername
sudo usermod -aG kvm yourusername
groups yourusername
```

3. 重启libvirt

```
sudo systemctl restart libvirtd
```

## 4 显卡直通

### 4.1 导出显卡的VGA BIOS

- Windows：[GPU-Z](https://www.techpowerup.com/gpuz/)
- Linux：[NVIDIA](https://www.techpowerup.com/download/nvidia-nvflash/) or [AMD](https://www.techpowerup.com/download/ati-atiflash/)
- [Tech Power Up](https://www.techpowerup.com/vgabios/)下载对应版本

Ubuntu中使用NVFlash导出Nvidia的rom文件，如果导出失败可以尝试将显卡驱动切换到Nouveau display driver，导出成功后再切换回来：

```
cd /path/to/nvflash/x64
./nvflash --save backup.rom
```

### 4.2 VBIOS补丁（只有英伟达显卡需要）

1. 安装十六进制编辑器

- Archliux

```
sudo pacman -S bless
```

- Ubuntu

我在Ubuntu中使用bless软件一直崩溃，最后使用了[ImHex](https://imhex.werwolv.net/)

2. 用十六进制编辑器打开导出的rom，查找字符VIDEO，找到VIDEO前面第一个大写U(十六进制是55)，删除U之前(包括U)的所有的内容，然后保存到`~/VM/vbios/patched.rom`。

3. 将修改后的文件保存到下面路径：

- Arch/Fedora保存到/var/lib/libvirt/vbios
- Ubuntu/OpenSUSE/Mint(用AppArmour的发行版)保存到/usr/share/vgabios

4. 修改权限：

```
sudo chmod 660 patched.rom
```

5. 更改拥有者：

```
sudo chown 你的用户名:users patched.rom
```

### 4.3 配置qemu虚拟机

1. 打开虚拟机管理器，把显卡的所有pci部分都添加到虚拟机里面。
2. 打开[虚拟系统管理器]->[编辑]->[首选项]->[常规]，勾选启用XML编辑, 
3. 编辑显卡的所有pci设备的XML, 在`</source>`之后添加:

```
<rom bar="on" file="/path/to/patched.rom"/>
```

3. 删除数位板、显示协议Spice、串口、信道spice、显卡QXL、USB转发器。
4. 添加你的USB设备，比如鼠标、键盘和USB耳机。
5. CPU->型号，可设置为host-passthrough。

### 4.4 其他XML配置

- 绕过英伟达Geforce显卡防虚拟化检测（2021四月之后好像不需要了）

在`</hyperv>`之前添加（value应为8-12个字母）：

```
<vendor_id state='on' value='randomid'/>
```

在`<features>`和`</features>`之间添加：

```
<kvm>
    <hidden state='on'/>
</kvm>
```

- 修复AMD cpu不支持超线程

在`</cpu>`前面加上：

```
<feature policy='require' name='topoext'/>
```

## 5 其他技巧

- 推荐在做显卡直通前安装openssh服务，以防出现黑屏时可以连接到电脑进行重启等操作：

```
sudo apt install openssh-server
```

其他操作：

```
sudo systemctl start sshd
sudo systemctl enable sshd
# 列出虚拟机
virsh list --all
# 关闭虚拟机
virsh shutdown win10
# 查找虚拟机进程
ps -ef | grep qemu-system
```

- 直通Ubuntu虚拟机后黑屏

通过ssh连接到虚拟机，安装显卡驱动，然后重启：

```
# 列出可以安装的驱动
ubuntu-drivers devices
# 安装显卡驱动
sudo apt install nvidia-driver-525
```

- 解决蓝牙连接不上

直通USB控制器，使用以下命令找出哪个PCI设备对应于哪个控制器以及每个端口和设备对应哪个控制器：

```
for usb_ctrl in $(find /sys/bus/usb/devices/usb* -maxdepth 0 -type l); do pci_path="$(dirname "$(realpath "${usb_ctrl}")")"; echo "Bus $(cat "${usb_ctrl}/busnum") --> $(basename $pci_path) (IOMMU group $(basename $(realpath $pci_path/iommu_group)))"; lsusb -s "$(cat "${usb_ctrl}/busnum"):"; echo; done
```

显示如下：

```
Bus 1 --> 0000:00:14.0 (IOMMU group 5)
Bus 001 Device 005: ID 04f3:0732 Elan Microelectronics Corp. 
Bus 001 Device 003: ID 05e3:0610 Genesys Logic, Inc. 4-port hub
Bus 001 Device 002: ID 1462:7d97 Micro Star International MYSTIC LIGHT 
Bus 001 Device 007: ID 8087:0033 Intel Corp. 
Bus 001 Device 004: ID 05e3:0608 Genesys Logic, Inc. Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

然后根据显示的PCI设备ID，如这里是0000:00:14.0，直接在虚拟机配置中添加相应的PCI主机设备。

- 报错：audio: Could not init `spice' audio driver

删除虚拟机xml配置中的下面这一行：

```
<audio id="1" type="spice"/>
```

- 报错：Could not access KVM kernel module: Permission denied

将用户加入kvm用户组

```
sudo usermod -aG kvm yourusername
```

- 报错：modprobe: FATAL: Module vfio_virqfd not found in directory /lib/modules/6.2.0-37-generic

vfio_virqfd在内核6.2版本之后不再支持。

modprobe加载的模块可以用如下命令查看：

```
# 查看vfio相关模块
lsmod | grep vfio
# 查看nvidia相关模块
lsmod | grep nvidia
```


#### 参考资源

- [【KVM实验室】单显卡直通 | Linux虚拟化保姆级教程](https://www.bilibili.com/video/BV1Er4y1i7pq)
- [LEDs-single-gpu-passthrough](https://gitlab.com/liucreator/LEDs-single-gpu-passthrough)
- [Libvirt](https://ubuntu.com/server/docs/virtualization-libvirt)
- [在KVM虚拟化中启用UEFI支持](https://cn.linux-console.net/?p=3254)
- [The Framebuffer Console - vtconsole](https://www.kernel.org/doc/html/latest/fb/fbcon.html)
- [Device passthrough in KVM](https://fenguoerbian.github.io/post/device-passthrough-in-kvm/)
- [PCI passthrough via OVMF](https://wiki.archlinuxcn.org/wiki/PCI_passthrough_via_OVMF)
