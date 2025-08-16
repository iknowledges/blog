# Clang使用GDB调试时字符串不显示

### 方法一：增加编译参数-fno-limit-debug-info

缺点：会使调试信息过长，降低链接和调试效率。

### 方法二：Ubuntu上安装libstdc++6-dbgsym

1. 导入签名

```
sudo apt install ubuntu-dbgsym-keyring
```

2. 创建ddebs.list文件

```
echo "deb http://ddebs.ubuntu.com $(lsb_release -cs) main restricted universe multiverse
deb http://ddebs.ubuntu.com $(lsb_release -cs)-updates main restricted universe multiverse
deb http://ddebs.ubuntu.com $(lsb_release -cs)-proposed main restricted universe multiverse" | \
sudo tee -a /etc/apt/sources.list.d/ddebs.list
```

3. 更新并安装包

```
sudo apt-get update
sudo apt install libstdc++6-dbgsym
```

#### 参考资料

- [Can't print std::string in (c)gdb](https://bugs.llvm.org/show_bug.cgi?id=24202)
- [Debug symbol packages](https://ubuntu.com/server/docs/debug-symbol-packages)
