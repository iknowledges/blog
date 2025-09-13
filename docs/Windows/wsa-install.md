# Windows Subsystem For Android安装教程

### 安装WSA

1. 下载安装包[WSA_2407.40000.4.0_x64_Release-Nightly-NoGApps-RemovedAmazon.7z
](https://github.com/MustardChef/WSABuilds/releases)并解压，找到Run.bat文件，双击运行。

2. 安装完成后打开【适用于Android的Windows子系统】，点击【高级设置】，打开【开发人员模式】。

### 安装adb

1. 打开官网[下载适用于Windows的SDKPlatform-Tools](https://developer.android.google.cn/tools/releases/platform-tools?hl=zh-cn)，解压得到platform-tools文件夹，将此文件夹添加到系统环境变量Path。

2. 测试adb是否安装成功

```
adb version
```

3. 安装apk

```
# 测试adb是否连接成功
adb devices
# 如果没连接成功，使用下面命令连接子系统
adb connect 127.0.0.1:58526
# 安装apk
adb install D:\download\apk\weixin.apk
```

#### 参考资料

- [安卓子系统WSA安装教程](https://www.bilibili.com/video/BV146YhzAErM/)
- [新手教程：如何安装Windows 11 安卓子系统](https://zhuanlan.zhihu.com/p/657679707)
