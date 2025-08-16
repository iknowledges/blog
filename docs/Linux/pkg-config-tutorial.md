# pkg-config使用教程

### 安装pkg-config

```
apt-get install pkg-config
```

### 常用命令

pkg-config查找信息的路径：

1. 系统/usr/lib下的*.pc文件
2. 环境变量PKG_CONFIG_PATH路径下的*.pc文件

```
# 默认搜索路径
pkg-config --variable pc_path pkg-config
# 查找头文件信息
pkg-config opencv --cflags
# 查找库文件信息
pkg-config opencv --libs
# 列出所有包
pkg-config --list-all
```

### 在CMake中使用pkg-config

CMake中有两种方法查找库：

- pkg_check_modules：根据列表中给的外部库，在当前环境下都试着去找到
- pkg_search_module：找到列表中第一个成功找到的外部库​

```
# 添加自定义的搜索路径
set(ENV{PKG_CONFIG_PATH} "/path/to/your/pkgconfig:$ENV{PKG_CONFIG_PATH}")

find_package(PkgConfig REQUIRED)

if (PKG_CONFIG_FOUND)
    pkg_check_modules(my_deps REQUIRED IMPORTED_TARGET libpng libwebp)
endif()

target_link_libraries(${PROJECT_NAME} PkgConfig::my_deps)
```

IMPORTED_TARGET参数将创建一个名为PkgConfig::${prefix}的导入目标，该目标可以作为参数直接传递给target_link_libraries()。

返回值：

- <XXX>_FOUND：如果模块存在，则设置为1
- <XXX>_LIBRARIES：只有库(没有“-l”)
- <XXX>_LINK_LIBRARIES：库及其绝对路径
- <XXX>_LIBRARY_DIRS：库的路径(没有“-L”)
- <XXX>_LDFLAGS：所有必需的链接器标志
- <XXX>_LDFLAGS_OTHER：所有其他链接器标志
- <XXX>_INCLUDE_DIRS：预处理器标记“-I”(没有“-I”)
- <XXX>_CFLAGS：所有必需的cflags
- <XXX>_CFLAGS_OTHER：其他编译器标志
- <YYY>_VERSION：模块版本
- <YYY>_PREFIX：模块的前缀目录
- <YYY>_INCLUDEDIR：包含模块的目录
- <YYY>_LIBDIR：模块的Lib目录

#### 参考资料

- [cmake：pkg_check_modules](https://blog.csdn.net/zhizhengguan/article/details/111826697)
- [使用 pkg-config 让 C++ 工程编译配置更灵活](https://zhuanlan.zhihu.com/p/417285806)
