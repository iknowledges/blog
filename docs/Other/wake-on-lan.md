# Wake On Lan网络唤醒

## 设置BIOS

- 【Advanced】->【Wake On LAN】->【Enabled】
- 【Boot】->【Fast Boot】->【Disabled】

## 设置网卡和电源

1. 找到正在使用的网卡，右键选择【属性】，然后点击【网络】->【配置】：

【电源管理】勾选【允许计算机关闭此设备以节约电源】、【允许此设备唤醒计算机】和【只允许幻数据包唤醒计算机】。

【高级】内的属性设置如下：

- 【Wake on Pattern Match】->【Enabled】
- 【Wake on Magic Packet】->【Enabled】

2. 【电源选项】->【选择电源按钮的功能】->取消勾选【启用快速启动】

## 设置防火墙

依次选择【Windows Defender 防火墙】->【高级设置】->【入站规则】->【新建规则】：

1. 【规则类型】选择【端口】；
2. 【协议和端口】选择UDP，并在特定本地端口输入9;
3. 【操作】选择【允许连接】；
4. 【配置文件】只勾选【公用】，取消勾选【域】和【专用】；
5. 【名称】输入自定义名称，如wol，然后点击完成。

## 测试软件

- [Fing](https://www.ghxi.com/fing.html)：发送唤醒指令
- [WakeOnLAN](https://github.com/basildane/WakeOnLAN/releases)：可以测试收到的数据包

#### 参考资料

- [网络唤醒wol详细教程，不需要路由器支持wol](https://www.bilibili.com/video/BV1ge4y1i7Xm)
- [怎么设置ToDesk远程开机](https://www.todesk.com/faq/57.html)
