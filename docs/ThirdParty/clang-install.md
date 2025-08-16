# Clang安装教程

## Ubuntu下直接用apt命令安装

```
sudo apt install clang
clang -v
```

## 使用自动安装脚本

```
wget https://apt.llvm.org/llvm.sh
chmod +x llvm.sh
# 18是Clang的版本号
sudo ./llvm.sh 18
# 设置Clang-18为系统默认版本，100表示优先级
sudo update-alternatives --install /usr/bin/clang clang /usr/bin/clang-18 100
sudo update-alternatives --install /usr/bin/clang++ clang++ /usr/bin/clang++-18 100
```

#### 参考资料

- [在 Ubuntu Linux 上安装 Clang](https://linuxstory.org/how-to-install-clang-on-ubuntu-linux/)
- [LLVM Debian/Ubuntu nightly packages](https://apt.llvm.org/)
- [https://zhuanlan.zhihu.com/p/496990553](https://zhuanlan.zhihu.com/p/496990553)
