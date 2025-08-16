# Ubuntu安装中文语言包

1. 安装中文语言包
```bash
# 搜索中文语言包
sudo apt-cache search language-pack-zh
# 安装简体中文语言包
sudo apt install language-pack-zh-hans
```

2. 配置中文系统语言
```bash
# 查看当前使用语言
locale
# 查看系统支持语言
locale -a
# 重新配置系统语言
sudo dpkg-reconfigure locales
```

#### 参考资料

[Change system language on Ubuntu 22.04 from command line](https://linuxconfig.org/change-system-language-on-ubuntu-22-04-from-command-line)
