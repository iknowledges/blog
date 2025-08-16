# zlib源码安装教程

1. 下载[zlib131.zip](https://github.com/madler/zlib/releases)源码并解压到`F:/third_party_build`目录。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/zlib-1.3.1
- Where to build the binaries: F:/third_party_build/zlib-1.3.1/build
- CMAKE_INSTALL_PREFIX: F:/third_party/zlib
- INSTALL_BIN_DIR: F:/third_party/zlib/bin
- INSTALL_INC_DIR: F:/third_party/zlib/include
- INSTALL_LIB_DIR: F:/third_party/zlib/lib
- INSTALL_MAN_DIR: F:/third_party/zlib/share/man
- INSTALL_PKGCONFIG_DIR: F:/third_party/zlib/share/pkgconfig

3. 用Visual Studio打开build目录下生成的zlib.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

#### 参考资料

- [zlib官网](https://zlib.net/)