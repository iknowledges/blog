# Ubuntu20禁用触屏

```
# 列出设备
xinput list
# 禁用触屏，这里也可以写设备id
xinput disable "ELAN Touchscreen"
# 启用触屏
xinput enable "ELAN Touchscreen"
```

## 启动时禁用触屏

1. 修改配置文件

```
sudo nano /usr/share/X11/xorg.conf.d/40-libinput.conf
```

2. 将`MatchIsTouchscreen "on"`修改为`MatchIsTouchscreen "off"`：

```
Section "InputClass"
        Identifier "libinput touchscreen catchall"
        MatchIsTouchscreen "off"
        MatchDevicePath "/dev/input/event*"
        Driver "libinput"
EndSection
```

3. 然后重启电脑

#### 参考资料

- [ubuntu 20 disable touchpad](https://juejin.cn/s/ubuntu%2020%20disable%20touchpad)
- [How do I disable the touchscreen drivers?](https://askubuntu.com/questions/198572/how-do-i-disable-the-touchscreen-drivers)
