# Windows和Ubuntu双系统的安装和卸载

## 下载Ubuntu系统镜像

https://cn.ubuntu.com/download/desktop

## 下载镜像写入工具Win32 Disk Imager

https://sourceforge.net/projects/win32diskimager/

## 制作Ubuntu安装盘

![windows-ubuntu-1.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-1.png)

## 磁盘分区

![windows-ubuntu-2.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-2.png)

打开磁盘管理，右键D盘，选择压缩卷，然后在`输入压缩空间量`输入40000MB，点击压缩。

![windows-ubuntu-3.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-3.png)

## 查看磁盘格式

Windows 10的硬盘分区格式有两种：MBR和GPT

![windows-ubuntu-4.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-4.png)

### 方法一：

打开磁盘管理，右键`磁盘0`的灰色区域，选择属性，在磁盘分区形式部分就可以看到相应的分区格式。

![windows-ubuntu-5.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-5.png)

### 方法二：

通过命令判断系统引导方式，进入Windows，在cmd中输入下面命令：

```
msinfo32
```

选择系统摘要，如果BIOS模式是传统，则说明系统是Legacy引导方式；如果是UEFI，则是UEFI引导方式。

## MBR分区安装Ubuntu

插入Ubuntu系统安装盘，重启电脑进入BIOS。

UEFI/Legacy Boot设置为Legacy Only，按F10保存并再次重启：

![windows-ubuntu-6.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-6.png)

重新开机按F12，选择从U盘启动就进入系统安装页面了。

安装类型选择其他选项：

![windows-ubuntu-7.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-7.png)

进行如下分区操作：

| 挂载点 | 新分区类型 | 用于| 大小 |
| --- | --- | --- | --- |
| /boot | 逻辑分区 | Ext4日志文件系统 | 500MB |
| 无 | 逻辑分区 | 交换空间 | 10000MB |
| / | 逻辑分区 | Ext4日志文件系统 | 20000MB |
| /home | 逻辑分区 | Ext4日志文件系统 | 剩余空间 |

![windows-ubuntu-8.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-8.png)

## GPT分区安装Ubuntu

进入BIOS，将操作系统类型设置为Windows UEFI模式：

![windows-ubuntu-9.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-9.png)

将系统启动顺序第一项设置为U盘：

![windows-ubuntu-10.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-10.png)

不同品牌电脑进入启动选项的按键：

![windows-ubuntu-11.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-11.png)

安装类型选择其他选项，进行如下分区操作：

| 挂载点 | 新分区类型 | 用于| 大小 |
| --- | --- | --- | --- |
| 无 | 主分区 | EFI系统分区 | 500MB |
| 无 | 主分区 | 交换空间 | 10000MB |
| / | 主分区 | Ext4日志文件系统 | 20000MB |
| /home | 主分区 | Ext4日志文件系统 | 剩余空间 |

注意：将`安装启动引导器的设备`和EFI系统分区类型的设备设置为同一个，否则无法引导进入Ubuntu。

一般来说可以按照如下规则设置swap大小：

- 4G以内的物理内存，SWAP 设置为内存的2倍，不超过4G。
- 4-8G的物理内存，SWAP 等于内存大小。
- 8-64G 的物理内存，SWAP 设置为8G。
- 64-256G物理内存，SWAP 设置为16G。

![windows-ubuntu-12.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-12.png)

## 系统时间同步

进入Ubuntu设置时间

```bash
sudo apt install ntpdate
# 通过互联网同步时间
sudo ntpdate time.windows.com
# 把时间机制从UTC改成Localtime
sudo hwclock --localtime --systohc
```

重启电脑进入Windows，可以看到时间已经是一致的了。

## 启动菜单的默认项

Ubuntu修改grub默认启动项

```bash
sudo gedit /etc/default/grub
```

如图修改成GRUB_DEFAULT=4，就可以默认启动Windows了：

![windows-ubuntu-13.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-13.png)

然后更新grub：

```bash
sudo update-grub
```

## Ubuntu安装盘恢复成普通U盘

安装[DiskGenius](https://www.diskgenius.cn/download.php)，然后插入U盘并启动软件。

右键空闲区域，选择建立新分区，点击确定：

![windows-ubuntu-14.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-14.png)

文件系统类型选择NTFS，其他参数保存默认，点击确定：

![windows-ubuntu-15.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-15.png)

最后点击工具栏左侧的保存更改。

## 一枚U盘实现多个版本系统的启动

下载[Venttoy](https://www.ventoy.net/cn/download.html)，插入U盘并启动软件，点击安装按钮即可开始制作启动U盘了，最后只需要将iso镜像文件拷贝到U盘的根目录：

![windows-ubuntu-16.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-16.png)

## 移除Legacy引导的Ubuntu，恢复Windows单系统

1. 首先要修复Windows系统引导：

下载[mbrfix.exe](https://sysint.no/en/mbrfix/)，将下载的压缩包解压，然后在解压的目录mbrfix打开cmd，如果是32位系统，就运行`MbrFix.exe`，如果是64位系统，就运行`MbrFix64.exe`

打开磁盘管理，查看要修复引导磁盘的序号，如下图系统安装在c盘，序号是0：

![windows-ubuntu-17.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-17.png)

右键点击`MbrFix64.exe`，选择属性->兼容性，勾选以管理员身份运行此程序。

![windows-ubuntu-18.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-18.png)

最后输入命令，重启电脑测试是否正常引导：

```
MbrFix64.exe /drive 0 fixmbr
```

2. 回收Ubuntu使用的磁盘空间，打开磁盘管理，右键点击没有显示分区格式的分区，选择删除卷。

## 移除UEFI引导的Ubuntu，恢复Windows单系统

下载安装[DiskGenius](https://www.diskgenius.cn/download.php)，打开软件，找到Ubuntu的EFI分区，一般和Ubuntu分区相邻的ESP就是Ubutnu的EFI分区，依次右键点击ESP(3)、分区(4)和分区(5)，点击删除当前分区，删除完成后点击工具栏的保存更改

![windows-ubuntu-19.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-19.png)

点击另一个ESP，找到EFI->ubuntu，如图彻底删除Ubuntu的引导项：

![windows-ubuntu-20.png](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Windows/windows-ubuntu-20.png)

#### 参考资源

[Windows和Ubuntu双系统的安装和卸载](https://www.bilibili.com/video/BV1554y1n7zv/)
[Recommended Partitioning Scheme](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/installation_guide/s2-diskpartrecommend-ppc#id4394007)
