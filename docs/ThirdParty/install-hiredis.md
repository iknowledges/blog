# hiredis源码编译安装教程

## Windows下安装

1. 下载[hiredis-1.3.0.zip](https://github.com/redis/hiredis/releases)源码并解压到`E:/third_party_build`目录。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: E:/third_party_build/hiredis-1.3.0
- Where to build the binaries: E:/third_party_build/hiredis-1.3.0/build_win
- CMAKE_INSTALL_PREFIX: E:/third_party/hiredis

3. 用Visual Studio打开build_win目录下生成的hiredis.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

4. CMakeLists.txt中使用hiredis

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR})

find_package(hiredis CONFIG REQUIRED)
message(STATUS "Using hiredis ${hiredis_VERSION}")

target_link_libraries(${PROJECT_NAME} PRIVATE hiredis::hiredis)

get_target_property(HiredisDll hiredis::hiredis IMPORTED_LOCATION_DEBUG)
add_custom_command(
    TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${HiredisDll} $<TARGET_FILE_DIR:${PROJECT_NAME}>
)
```

## Ubuntu下安装

1. 编译项目

```
git clone -b v1.2.0 https://github.com/redis/hiredis.git
cmake -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR/hiredis -DCMAKE_BUILD_TYPE=Debug ..
make -j 6
make install
```

2. CMakeLists.txt中使用hiredis

```
list(APPEND CMAKE_PREFIX_PATH $ENV{MY_INSTALL_DIR})

find_package(hiredis CONFIG REQUIRED)
include_directories(${hiredis_INCLUDE_DIRS})

target_link_libraries(${PROJECT_NAME} PRIVATE ${hiredis_LIBRARIES})
```

测试代码main.cpp如下：

```cpp
#include <iostream>
#include <hiredis/hiredis.h>

int main(int argc, char const* argv[]) {
    redisContext* c = redisConnect("localhost", 6379);
    if (c->err) {
        std::cerr << "Connect to Redis Server failed: " << c->errstr << std::endl;
        redisFree(c);
        return -1;
    }
    std::cout << "Connect to Redis Server Success" << std::endl;
    redisFree(c);
    return 0;
}
```

#### 参考资料

- [hiredis CMakeLists.txt](https://github.com/redis/hiredis/blob/master/CMakeLists.txt)