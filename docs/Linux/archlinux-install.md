# ArchLinux安装教程

## 1 安装前准备

### 1.1 获取安装镜像

下载地址：https://archlinux.org/download/

### 1.2 配置控制台字体

```
setfont /usr/share/kbd/consolefonts/LatGrkCyr-12x12.psfu.gz
```

### 1.3 连接wifi（可选）

```
ip link
ip link set wlan0 up
iwlist wlan0 scan | grep ESSID
wpa_passphrase 网络 密码 > internet.conf
wpa_supplicant -c internet.conf -i wlan0 &
dhcpcd &
ping archlinux.org
```

### 1.4 更新系统时间

```
# 将系统时间与网络时间进行同步
timedatectl set-ntp true
# 检查服务状态
timedatectl status
```

### 1.5 磁盘分区

1. 创建磁盘分区

```
# 查看设备
fdisk -l
# 创建磁盘分区，类型选择gpt
cfdisk /dev/sda
```

2. 格式化分区

```
# 主分区
mkfs.ext4 /dev/sda3
# 引导分区
mkfs.fat -F32 /dev/sda1
# swap分区
mkswap /dev/sda2
```

3. 挂载分区

```
# 主分区
mount /dev/sda3 /mnt
# 引导分区
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
# swap分区
swapon /dev/sda2
```

## 2 开始安装系统

### 2.1 选择镜像站

1. 编辑`/etc/pacman.d/mirrorlist`，在文件的最顶端添加：

```
Server = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
```

更新软件包缓存，两次y能避免从损坏的镜像切换到正常的镜像时出现的问题：

```
pacman -Syyu
```

如果您从一个较新的镜像切换到较旧的镜像，以下命令可以降级部分包，以避免系统的部分更新：

```
pacman -Syyuu
```

2. 在`/etc/pacman.conf`文件末尾添加两行：

```
[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

之后通过以下命令安装archlinuxcn-keyring包导入GPG key：

```
sudo pacman -Sy archlinuxcn-keyring
```

可选操作：打开pacman.conf中的Color注释可启动命令窗口的颜色。

### 2.2 安装必需软件

1. 安装base软件包和Linux内核以及常规硬件的固件

```
pacstrap /mnt base linux linux-firmware
```

2. 安装其他软件

- 必需软件

```
pacstrap /mnt networkmanager vim openssh
```

- wifi相关软件（可选）

```
pacstrap /mnt wpa_supplicant dhcpcd
```

## 3 配置系统

### 3.1 生成fstab文件

fstab用来定义磁盘分区。

```
genfstab -U /mnt >> /mnt/etc/fastab
# 检查生成的文件是否有错误
cat /mnt/etc/fastab
```

### 3.2 chroot到新安装的系统

```
arch-chroot /mnt
```

### 3.3 设置时区

```
# 设置时区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
# 生成/etc/adjtime
hwclock --systohc
```

### 3.4 区域和本地化设置

1. 设置locale.gen配置文件

```
vim /etc/locale.gen
```

将下面注释放开：

```
...
en_US.UTF-8 UTF-8
...
```

2. 执行locale-gen以生成locale信息

```
locale-gen
```

3. 然后创建locale.conf文件

```
vim /etc/locale.conf
```

设定LANG变量：

```
LANG=en_US.UTF-8
```

### 3.5 网络配置

1. 设置主机名

```
vim /etc/hostname
```

内容如下：

```
myarch
```

2. 设置/etc/hosts，用Tab键对齐

```
vim /etc/hosts
```

内容如下：

```
127.0.0.1   localhost
::1         localhost
127.0.0.1   myarch.localdomain  myarch
```

### 3.6 设置root密码

```
passwd
```

### 3.7 安装引导程序

1. 安装相应的包

```
pacman -S grub efibootmgr intel-ucode os-prober
```

- grub：启动引导器
- efibootmgr：efibootmgr被grub脚本用来将启动项写入NVRAM
- os-prober：如果安装双系统，需要安装os-prober以检测Windows
- intel-ucode：芯片制造商的微码，Intel芯片安装intel-ucode，AMD芯片安装amd-ucode

2. 安装GRUB到EFI分区

```
uname -m
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ARCH
```

3. 生成配置文件

```
mkdir /boot/grub
grub-mkconfig -o /boot/grub/grub.cfg
```

## 4 重新启动计算机

```
# 退回安装环境
exit
# 卸载新分区
umount -R /mnt
# 关闭wifi（可选）
killall wpa_suppicant dhcpcd
# 重启
reboot
```

重启后登录root用户并设置开机自启网络服务：

```
systemctl enable --now NetworkManager
```

#### 参考资源

- [ArchLinux安装指南](https://wiki.archlinuxcn.org/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97)
- [archlinux基础安装](https://arch.icekylin.online/guide/rookie/basic-install)
- [USTC Mirror Help](https://mirrors.ustc.edu.cn/help/archlinux.html)
- [Arch Linux软件仓库](https://mirrors.tuna.tsinghua.edu.cn/help/archlinux/)
