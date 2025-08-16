# spdlog源码编译安装教程

## Windows下安装

1. 下载[spdlog-1.15.1.zip](https://github.com/gabime/spdlog/releases)源码并解压到`F:/third_party_build`目录。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/spdlog-1.15.1
- Where to build the binaries: F:/third_party_build/spdlog-1.15.1/build
- CMAKE_INSTALL_PREFIX: F:/third_party/spdlog

3. 用Visual Studio打开build目录下生成的spdlog.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成，完成后spdlog的头文件和库文件就会安装到CMAKE_INSTALL_PREFIX指定目录下。

## Linux下安装

1. 进入spdlog的源代码目录执行下面命令：

```
ccmake -S . -B ./build_linux -G "Unix Makefiles"
```

2. 输入c，修改完配置，再输入g：

- CMAKE_INSTALL_PREFIX: /path/to/third_party_linux/spdlog
- SPDLOG_BUILD_PIC: ON

3. 编译并安装：

```
cmake --build ./build_linux
cmake --install ./build_linux
```

### 编写CMakeLists.txt

```
cmake_minimum_required(VERSION 3.11)
project(spdlog_examples)

list(APPEND CMAKE_PREFIX_PATH F:/third_party)

find_package(spdlog REQUIRED)

add_executable(example example.cpp)
target_link_libraries(example PRIVATE spdlog::spdlog)
```

#### 参考资料

- [spdlog库笔记 ：编译、安装](https://blog.csdn.net/qq_28368377/article/details/130872394)