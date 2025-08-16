# Ubuntu编译MySQL Server

1. 设置环境变量

```
export MY_INSTALL_DIR=$HOME/.local
export PATH="$MY_INSTALL_DIR/mysql-server/bin:$PATH"
```

2. 下载官网源码包[mysql-8.4.0.tar.gz](https://dev.mysql.com/downloads/mysql/)，下拉框选择Source Code和All Operating Systems (Generic)，github下载的代码可能不稳定，最好选择在官网下载。

```
tar zxvf mysql-8.4.0.tar.gz
cd mysql-8.4.0
```

3. 安装Curses

```
sudo apt install libncurses5-dev
```

4. 预先配置

```
groupadd mysql
useradd -r -g mysql -s /bin/false mysql
```

5. 编译安装

```
mkdir bld
cd bld
cmake -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR/mysql-server -DWITH_BOOST=/path/to/boost_1_77_0 -DWITH_SSL=$MY_INSTALL_DIR/OpenSSL ..
make -j 8
make install
```

- WITH_BOOST: 是指Boost源代码路径，不是安装路径
- WITH_SSL: 是指OpenSSL的安装路径，默认是system，即系统OpenSSL，可通过命令`apt install libssl-dev`安装

6. 启动mysql

```
# 初始化生成密码，并在当前目录生成data文件夹，重新初始化需要删除data文件夹
bin/mysqld --initialize --user=mysql
# 启动mysql服务
bin/mysqld_safe --user=mysql &
```

#### 错误处理

- 报错信息如下：

```
CMake Error at CMakeLists.txt:1833 (MESSAGE):
  Please install the patchelf(1) utility.
```

解决方法：

```
sudo apt install patchelf
```

#### 参考资料

- [2.8 Installing MySQL from Source](https://dev.mysql.com/doc/refman/8.0/en/source-installation.html)