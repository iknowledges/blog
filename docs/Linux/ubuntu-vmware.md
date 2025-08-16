# Ubuntu中Vmware安装虚拟机问题

## 报错信息

使用Vmware安装Windows10报错：**Cannot open /dev/vmmon: No such file or directory**

## 解决方法

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=VMware/"
sudo /usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vmmon)
sudo /usr/src/linux-headers-`uname -r`/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vmnet)
sudo mokutil --import MOK.der
# 如果导入失败就使用下面的命令重置密码
sudo mokutil --password
# 重启电脑
sudo reboot
```

重启电脑时perform mok management，出现了蓝屏的MOK management，具体的解决办法如下：

1. **当进入蓝色背景的界面perform mok management 后，选择 enroll mok ,**
2. **进入enroll mok 界面，选择 continue ,**
3. **进入enroll the key 界面，选择 yes ,**
4. **接下来输入你在安装驱动时输入的密码，**
5. **之后会跳到蓝色背景的界面perform mok management 选择第一个 reboot。**

## 参考资料

[Cannot open /dev/vmmon: No such file or directory](https://kb.vmware.com/s/article/2146460?lang=zh_cn)