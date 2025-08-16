# Ubuntu编译JsonCpp

1. 下载源码

```
git clone -b 1.9.5 https://github.com/open-source-parsers/jsoncpp.git
cd jsoncpp
```

2. 编译安装

```
mkdir -p build/debug
cd build/debug
cmake -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR/jsoncpp -DCMAKE_BUILD_TYPE=debug -DBUILD_STATIC_LIBS=ON -DBUILD_SHARED_LIBS=OFF -DARCHIVE_INSTALL_DIR=. -G "Unix Makefiles" ../..
make
```

3. CMakeLists.txt中引入jsoncpp：

```
list(APPEND CMAKE_PREFIX_PATH $ENV{MY_INSTALL_DIR})

find_package(jsoncpp CONFIG REQUIRED)
message(STATUS "Using JsonCpp ${jsoncpp_VERSION}")
get_target_property(JSON_INC_PATH jsoncpp_object INTERFACE_INCLUDE_DIRECTORIES)
include_directories(${JSON_INC_PATH})

target_link_libraries(${PROJECT_NAME} jsoncpp_object)
```

#### 参考资料

- [Building and testing with CMake](https://github.com/open-source-parsers/jsoncpp/wiki/Building)
