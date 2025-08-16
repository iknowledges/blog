# Docker中安装xfce

1. 创建docker容器

```bash
# 客户端可以通过<host_ip>:3390远程连接到容器
sudo docker run --name myubt -itd -p 3390:3389 --privileged ubuntu:jammy
sudo docker exec -it myubt bash
```

2. 安装桌面环境

```bash
apt install -y vim sudo
# 桌面环境
sudo apt install -y xfce4
# 远程连接工具
sudo apt -y install xrdp
# 启动服务
sudo service xrdp start
```

3. 安装默认终端
```bash
sudo apt-get -y install xfce4-terminal
sudo update-alternatives --config x-terminal-emulator
```

4. 安装浏览器
```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb 
sudo apt -f install
google-chrome --no-sandbox
```

5. 安装中文输入法

```bash
sudo apt install locales
sudo dpkg-reconfigure locales
```
选择语言编码en_US.UTF8，zh_CN GB2312，zh_CN GBK GBK，zh_CN UTF-8 UTF-8

在~/.bashrc最后一行添加
```bash
export LANG=zh_CN.UTF-8
```

安装中文字体
```bash
sudo apt install fonts-wqy-zenhei
```

## 注：最后运行浏览器时，刷新网页还是会报错，打开网页就崩溃

## 参考链接

[xfce中文字符方块乱码问题解决](https://blog.csdn.net/weixin_42937217/article/details/121970539)