# WSL安装ArchLinux

## 安装ArchLinux

### 方法一：

1. 去[ArchWSL](https://github.com/yuk7/ArchWSL/releases/latest)下载.appx和.cer文件。
2. 双击.cer文件，点击【安装证书】，【存储位置】选择【本地计算机】，点击【下一步】，然后选择【将所有的证书都放入下列存储】，点击【浏览】，选择【受信任人】，最后依次点击【确认】并【完成】。
3. 双击.appx文件安装子系统。

### 方法二：

1. 在Ubuntu系统中打包rootfs，参考https://wiki.archlinux.org/title/Full_system_backup_with_tar

```
# 安装压缩工具包
sudo apt install libarchive-tools
# 下载文件
wget https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/latest/archlinux-bootstrap-x86_64.tar.zst
# 解压
sudo bsdtar -xpvf archlinux-bootstrap-x86_64.tar.zst
# 重新打包为tar文件
sudo bsdtar -cpvf archlinux-bootstrap.tar -C root.x86_64 .
```

2. 在Windows中导入archlinux-bootstrap.tar为ArchLinux分发版本

```
wsl --import Arch D:\WSL\Arch D:\archlinux-bootstrap.tar
# 设置为默认分发版本
wsl --set-default Arch
```

## 配置ArchLinux

- 设置locale

```
sed -i -e "s/^#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/" /etc/locale.gen
locale-gen
echo 'LANG=en_US.UTF-8' > /etc/locale.conf
```

- 配置pacman镜像，参考https://wiki.archlinux.org/title/Pacman/Package_signing

```
echo 'Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch' > /etc/pacman.d/mirrorlist
echo -e '[archlinuxcn]\nServer = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch' >> /etc/pacman.conf
# 初始化密钥
pacman-key --init
# 验证密钥
pacman-key --populate
# 更新缓存
pacman -Syyu
pacman -Sy archlinuxcn-keyring
```

- 安装必需软件

```
pacman -S --needed vim sudo base-devel gdb cmake
```

- 配置用户

1. 打开sudo配置：

```
EDITOR=vim visudo
```

打开下面的注释：

```
## Uncomment to allow members of group wheel to execute any command
%wheel ALL=(ALL:ALL) ALL
```

2. 新建abc用户：

```
# 新增用户并加入wheel用户组
useradd abc -m -G wheel -s /bin/bash
# 设置用户密码
passwd abc
# 设置默认用户
echo -e "[user]\ndefault = abc" >> /etc/wsl.conf
```

3. 最后重启wsl：

```
wsl --shutdown
```

## 导出ArchLinux

```
wsl --export Arch archlinux-backup.tar
wsl --import Arch D:\WSL\Arch archlinux-backup.tar
```

#### 参考资源

- [ArchWSL Documentation](https://wsldl-pg.github.io/ArchW-docs/How-to-Setup/)
- [ArchLinux-WSL2-Install-Scripts](https://github.com/wswind/ArchLinux-WSL2-Install-Scripts)
- [WSL2 安装 ArchLinux —— In The Arch Way](https://zhuanlan.zhihu.com/p/613738433)
- [在WSL2中安装ArchLinux](https://blog.yurzi.net/posts/archlinux-on-wsl2/)
