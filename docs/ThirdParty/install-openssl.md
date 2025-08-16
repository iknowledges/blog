# Ubuntu编译OpenSSL

- 官网下载[openssl-3.0.14.tar.gz](https://www.openssl.org/source/)，并解压：

```
tar -zxvf openssl-3.0.14.tar.gz
cd openssl-3.0.14/
```

- **编译方法一**：通过设置环境变量LD_LIBRARY_PATH，指定链接器在运行时寻找共享库的路径

1. 编译安装

```
./Configure --prefix=$MY_INSTALL_DIR/OpenSSL --openssldir=$MY_INSTALL_DIR/OpenSSL
make
make install
```

2. 设置环境变量

```
export PATH="$MY_INSTALL_DIR/OpenSSL/bin:$PATH"
export LD_LIBRARY_PATH="$MY_INSTALL_DIR/OpenSSL/lib64:$LD_LIBRARY_PATH"
```

3. 测试是否安装成功

```
openssl version
```

- **编译方法二**：通过设置可执行文件的DT_RPATH或DT_RUNPATH，来指定程序运行时链接器寻找共享库的路径

1. 编译安装

```
./Configure --prefix=$MY_INSTALL_DIR/OpenSSL --openssldir=$MY_INSTALL_DIR/OpenSSL '-Wl,-rpath,$(LIBRPATH)'
make
make install
```

2. 设置环境变量

```
export PATH="$MY_INSTALL_DIR/OpenSSL/bin:$PATH"
```

3. 执行如下命令查看可执行文件的链接信息

```
readelf -d bin/openssl | grep RPATH
readelf -d bin/openssl | grep RUNPATH
```

#### 参考资料

- [Build and Install](https://github.com/openssl/openssl/blob/master/INSTALL.md)
- [Notes for UNIX-like platforms](https://github.com/openssl/openssl/blob/master/NOTES-UNIX.md)
