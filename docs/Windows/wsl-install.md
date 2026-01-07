# wsl安装教程

## 安装

1. 启用适用于Linux的Windows子系统，以管理员身份打开PowerShell，然后输入以下命令：

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

2. 启用虚拟机功能，以管理员身份打开PowerShell并运行，重新启动计算机：

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

3. 下载Linux内核更新包，通过命令`systeminfo | find "系统类型"`来查看计算机的类型

- [适用于x64计算机的WSL2 Linux内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
- [适用于arm64计算机的WSL2 Linux内核更新包](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi)
- 内核包也可以在Github下载[wsl.2.6.1.0.x64.msi或wsl.2.6.1.0.arm64.msi](https://github.com/microsoft/WSL/releases)

4. 将WSL 2设置为默认版本

```
wsl --set-default-version 2
```

5. 打开[旧版WSL的手动安装步骤](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual)页面，找到步骤6下面的【下载发行版】，点击链接来下载Linux发行版，双击AppxBundle文件即可安装。

## 迁移

```
# 查看有效分发版的列表
wsl --list --online
# 安装指定分发版
wsl --install Ubuntu-24.04
# 查看已安装的wsl状态
wsl -l -v
# 停止正在运行的wsl
wsl --shutdown
# 导出成tar包
wsl --export Ubuntu D:\ubuntu-backup.tar
# 卸载旧的wsl
wsl --unregister Ubuntu
# 导入tar包到指定位置
wsl --import Ubuntu D:\WSL\Ubuntu22 D:\ubuntu-backup.tar --version 2
# 恢复之前的用户
Ubuntu config --default-user <用户名>
```

- `wsl --update`下载过慢优化：【打开Windows更新】->【高级选项】->【传递优化】，【允许从以下来源下载】选择【Internet和我的本地网络上的设备】。

## 常用命令

- 查看WSL在Windows中的位置，使用PowerShell执行下面命令：

```
Get-ChildItem -Path "$env:LOCALAPPDATA\Packages" | Where-Object { $_.Name -match "Ubuntu|Debian|Fedora" }
```

- 导入和导出发行版

```
wsl --export <Distribution Name> <FileName>
wsl --import <Distribution Name> <InstallLocation> <FileName>
```

- 运行指定的Linux发行版

```
wsl --distribution <Distribution Name> --user <User Name>
```

## 其他问题

- 控制台中文乱码：

使用下面命令打开编码设置界面，然后空格键选中en_US.UTF-8，使用Tab键切换至OK，再将en_US.UTF-8选为默认，然后重启WSL。

en_US.UTF-8和zh_CN.UTF-8的区别是：系统的菜单、程序的工具栏语言、输入法默认语言前者是英文，后者是中文。

```
# 修改编码
sudo dpkg-reconfigure locales
# 查看当前编码
locale
```

- wsl: 检测到 localhost 代理配置，但未镜像到 WSL。NAT 模式下的 WSL 不支持 localhost 代理。

打开应用【WSL Settings】，找到【网络】->【网络模式】，将【Nat】换成【Mirrored】，然后重启WSL。

#### 参考资源

- [如何使用WSL在Windows上安装Linux](https://learn.microsoft.com/zh-cn/windows/wsl/install)
- [WSL的基本命令](https://learn.microsoft.com/zh-cn/windows/wsl/basic-commands)
- [解决win10的wsl2下Ubuntu系统里中文乱码问题](https://blog.csdn.net/weixin_39246554/article/details/123487843)
- [WSL Install (适用于windows的Linux子系统)下载速度提升](https://www.bilibili.com/opus/989978313838559256)
- [彻底卸载WSL步骤整理](https://zhuanlan.zhihu.com/p/1955953998516446643)
- [Where Are WSL2 Files Physically Stored? [And How to Relocate Them]](https://linuxvox.com/blog/where-are-the-files-inside-wsl2-physically-stored/)
