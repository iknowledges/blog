# Windows使用Samba服务共享文件

## 开启Samba服务

打开【Control Panel】->【Programs】->【Turn Windows features on or off】，勾选【SMB 1.0/CIFS File Sharing Support】，然后重启电脑。

## 网络设置

1. 点击【Network & internet】->【Advanced network settings】【Advanced sharing settings】，开启【Private networks】下的【Network discovery】和【File and printer sharing】，并勾选【Set up network connected devices automatically】

2. （可选）为了安全建议关闭【Public networks】下的【Network discovery】和【File and printer sharing】，关闭【All networks】下的【Public folder sharing】，开启【Password protexted sharing】

## 设置共享文件夹

1. 选择一个文件夹，右键->【properties】->【Sharing】，点击【Advanced Sharring】
2. 勾选【Share this folder】，点击【Permissions】，选择【Everyone】，勾选【Read】，最后点击【OK】
3. 在文件资源管理器中打开`\\localhost`，查看是否有刚刚共享的文件夹。
4. 如果需要取消共享就去除【Share this folder】的勾选。

## iphone连接

打开【Files】，点击右上角的三个点，打开【Connect to Server】，输入`smb://<你的IP地址>`，选择【Registered User】，然后输入你的登录用户和密码进行连接。

## VLC for Android连接

打开【Browse】，点击右上角的三个点，打开【Add a server favorite】，选择【SMB】，然后输入你的IP进行连接。

#### 参考资料

- [教你用手机／平板，直接播放电脑上的视频](https://zhuanlan.zhihu.com/p/45967353)
- [Windows11 文件夹共享设置 如何设置 如何访问（图文直观版）](https://zhuanlan.zhihu.com/p/421631878)