# Sunshine和Moonlight串流教程

## 安装Sunshine

1. 打开[Sunshine地址](https://github.com/LizardByte/Sunshine)下载exe程序并安装，选项安装的组件时可以去掉Scripts下的Virtual Gamepad选项，因为这个选项会检测虚拟手柄驱动，没有检测到的话安装可能会卡住不动。
2. 安装完成会弹出网页设置初始用户名和密码，设置完后再进行登录。

## 安装Moonlight

1. 下载[阿西西版的Moonlight](https://github.com/Axixi2233/moonlight-android)，因为它扩展了Android端的触控功能。
2. 手机端添加串流电脑的IP地址，会弹出PIN码，打开电脑端Sunshine，输入PIN码进行配对，设备名称随意。
3. 此时手机可以显示电脑桌面了，但是关掉显示器手机也会黑屏，所以下一步安装虚拟显示器。

## 安装虚拟显示器

1. 下载[ParsecVDisplay](https://github.com/nomi-san/parsec-vdd)的安装包，我选择的最新版v0.45.1，安装完成后设备管理器中的显示适配器会新增一个设备，如Parsec Virtual Display Adapter。
2. 打开Moonlight手机端，视频分辨率设置为原生全屏（2340x1080），视频帧数设置为60FPS。
3. 打开ParsecVDisplay，点击CUSTOM DISPLAY MODES，将Slot1设置为：2340x1080@60Hz，然后应用，返回后点击ADD DISPLAY。
4. 点击新增的DISPLAY，Resolution选择2340x1080，并记下\\.\DISPLAY6。
5. 打开Sunshine设置网页，找到Configuration->Audio/Video，将Output Name设置为\\.\DISPLAY6，然后保存。

## 安装虚拟音频驱动

1. 如果安装了Steam可以不用安装音频驱动。否则可以下载[SteamLinkAudioDrivers](https://github.com/AshVance/SteamLinkAudioDrivers)。
2. 打开【计算机管理】，选择【音频输入和输出】，点击【操作】->【添加过时硬件】，选择【安装我手动从列表选择的硬件】并点击下一步，选择【显示所有设备】并点击下一步，点击【从硬盘安装】，找到文件SteamLinkAudioDrivers/Steam VAC/drivers/Windows10/x64/SteamStreamingSpeakers.inf，再选择【Steam Streaming Speakers】并点击下一步，最后点击完成。
3. 在【设备管理器】的【音频输入和输出】中找到刚刚添加的【Steam Streaming Speakers】，右键选择属性，点开【详细信息】，属性选择【设备实例路径】，右键【值】点击复制。
4. 打开Sunshine设置网页，找到Configuration->Audio/Video，将刚刚复制的路径粘贴到【Virtual Sink】，删除前缀SWD\MMDEVAPI\，留下{数字}.{数字}，最后进行保存。

## HDMI显卡欺骗器

1. 使用显卡欺骗器后就不需要安装虚拟显示器ParsecVDisplay了，首先将显卡欺骗器插入主机。
2. 然后找到Sunshine安装目录下的tools文件夹，打开cmd命令行窗口运行dxgi-info.exe，就能看到需要填入Sunshine的Output Name。
3. 为了防止重启电脑后Moonlight找不到显示器，进行如下设置：在桌面右键选择显示设置，将模式选择为仅在真实的显示器上显示，如设置为【仅在1上显示】。如果选择【复制这些显示器】，熄屏后进行串流，显示器就无法自动切换。

#### 参考资料

- [所有平板/手机都能“运行”Windows？从零开始玩儿串流！](https://b23.tv/zp0BtYH)
- [Moonlight官网](https://moonlight-stream.org/)
- [Sunshine + Moonlight 熄屏串流方式](https://www.bilibili.com/read/cv30603647/)
