# nanomsg源码编译安装教程

## Windows下安装

1. 下载[nanomsg-1.2.zip](https://github.com/nanomsg/nanomsg/releases)源码并解压。

注：nanomsg-1.2.1版本有这个[bug](https://github.com/nanomsg/nanomsg/issues/1111)

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/nanomsg-1.2
- Where to build the binaries: F:/third_party_build/nanomsg-1.2/build
- CMAKE_INSTALL_PREFIX: F:/third_party/nanomsg

3. 用Visual Studio打开build目录下生成的nanomsg.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

## CMakeList.txt中使用nanomsg

```
list(APPEND CMAKE_PREFIX_PATH $ENV{THIRD_PARTY_DIR})

find_package(nanomsg CONFIG REQUIRED)
message(STATUS "Using nanomsg ${nanomsg_VERSION}")

target_link_libraries(${PROJECT_NAME} nanomsg)

add_custom_command(
    TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${nanomsg_BINDIR}/nanomsg.dll $<TARGET_FILE_DIR:${PROJECT_NAME}>
)
```

#### 参考资料

- [旧版本nanomsg](https://github.com/nanomsg/nanomsg)
- [新版本nanomsg](https://github.com/nanomsg/nng)
