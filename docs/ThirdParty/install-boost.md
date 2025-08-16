# Boost源码安装教程

## Ubuntu下安装

1. 下载boost安装包然后解压

```bash
wget https://archives.boost.io/release/1.77.0/source/boost_1_77_0.tar.bz2
tar --bzip2 -vxf boost_1_77_0.tar.bz2
```

2. 编译安装

```bash
cd boost_1_77_0/
# 生成配置
./bootstrap.sh --prefix=$THIRD_PARTY_DIR/Boost
# 查看生成的配置
cat project-config.jam
# 开始安装
./b2 install
```

#### 安装Boost.Python

```bash
./bootstrap.sh --prefix=$THIRD_PARTY_DIR/Boost --with-python=/usr/bin/python3
./b2 install --with-python
```

#### 卸载

删除以下文件：[PREFIX]/lib/libboost* 和 [PREFIX]/include/boost*

## Windows下安装

1. 下载[boost_1_87_0.zip](https://www.boost.org/users/history/version_1_87_0.html)，并解压文件。

2. 编译安装

```
cd boost_1_87_0
bootstrap.bat
.\b2 install --prefix=F:\third_party\Boost
```

## CMakeList.txt中使用Boost

```bash
list(APPEND CMAKE_PREFIX_PATH $ENV{THIRD_PARTY_DIR})

find_package(Boost REQUIRED COMPONENTS system)
message(STATUS "Using boost ${Boost_VERSION}")

target_include_directories(${PROJECT_NAME} PRIVATE ${Boost_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME} PRIVATE ${Boost_LIBRARIES})
```

#### 参考资料

- [Getting Started on Unix Variants](https://www.boost.org/doc/libs/1_77_0/more/getting_started/unix-variants.html)
- [How can I uninstall Boost with bjam?](https://lists.boost.org/boost-users/2005/01/9444.php)
- [Getting Started on Windows](https://www.boost.org/doc/libs/1_87_0/more/getting_started/windows.html)
