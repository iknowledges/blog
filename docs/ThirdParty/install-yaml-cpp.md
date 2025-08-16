# yaml-cpp源码编译安装教程

## Windows下安装

1. 下载[yaml-cpp-0.8.0.zip](https://github.com/jbeder/yaml-cpp/releases)源码并解压。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/yaml-cpp-0.8.0
- Where to build the binaries: F:/third_party_build/yaml-cpp-0.8.0/build
- CMAKE_INSTALL_PREFIX: F:/third_party/YAML_CPP

3. 用Visual Studio打开build目录下生成的YAML_CPP.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

## CMakeList.txt中使用yaml-cpp

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR}/YAML_CPP)

# yaml-cpp
find_package(yaml-cpp CONFIG REQUIRED)
message(STATUS "Using yaml-cpp ${yaml-cpp_VERSION}")

target_include_directories(${PROJECT_NAME} PRIVATE ${YAML_CPP_INCLUDE_DIR})
target_link_libraries(${PROJECT_NAME} PRIVATE yaml-cpp::yaml-cpp)
```

#### 参考资料

- [Tutorial](https://github.com/jbeder/yaml-cpp/wiki/Tutorial)
