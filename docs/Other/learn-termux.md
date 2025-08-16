# Termux教程

## Termux修改国内源

修改`/data/data/com.termux/files/usr/etc/apt/sources.list`，内容如下：

```
deb https://mirrors.ustc.edu.cn/termux/apt/termux-main stable main
```

或者使用命令替换文件内容：

```bash
sed -i 's@packages.termux.org@mirrors.ustc.edu.cn/termux@' $PREFIX/etc/apt/sources.list
```

然后更新源：

```bash
pkg update
```

## 常用设置

```
# 生成$HOME/storage路径以访问本地资源
termux-setup-storage
# 安装文件管理器
pkg install ranger
```

#### 参考资源

- [Termux-setup-storage](https://wiki.termux.com/wiki/Termux-setup-storage)
- [Termux配置neovim IDE](https://blog.csdn.net/lxyoucan/article/details/120066312)
