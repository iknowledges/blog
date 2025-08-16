# CMake配置第三方库talib

## Windows

1. 下载预编译的[ta-lib-0.6.4-windows-x86_64.zip](https://github.com/ta-lib/ta-lib/releases)文件，并解压到指定目录thirdparty下。

2. 新建一个项目，目录结构如下所示：

```
.
├── cmake
│   └── FindTalib.cmake
├── CMakeLists.txt
└── main.cpp
```

3. 编写FindTalib.cmake

```cmake
if(NOT DEFINED Talib_ROOT)
    set(Talib_ROOT ${CMAKE_CURRENT_LIST_DIR})
endif()

# 查找头文件
find_path(
    Talib_INCLUDE_DIR
    NAMES ta_abstract.h ta_common.h ta_defs.h ta_func.h ta_libc.h
    PATHS ${Talib_ROOT}/include
    NO_DEFAULT_PATH  # 默认路径下会包含/usr/local等内容，这句话表示不包含默认路径
)

# 查找静态库文件
find_library(
    Talib_STATIC_LIB
    NAMES ta-lib.lib ta-lib-static.lib
    PATHS ${Talib_ROOT}/lib
    NO_DEFAULT_PATH
)

# 查找动态库文件
find_file(
    Talib_DYNAMIC_LIB
    NAMES ta-lib.dll
    PATHS ${Talib_ROOT}/bin
    NO_DEFAULT_PATH
)

# 设置导入目标
if(NOT TARGET Talib_SDK)
    # 将目标设置为在项目中全局可见GLOBAL
    add_library(Talib_SDK STATIC IMPORTED GLOBAL)
    # 为导入目标添加相关属性
    set_target_properties(Talib_SDK PROPERTIES
        INTERFACE_INCLUDE_DIRECTORIES "${Talib_INCLUDE_DIR}"
        IMPORTED_LOCATION "${Talib_STATIC_LIB}"
    )
endif()

set(Talib_INCLUDE_DIRS ${Talib_INCLUDE_DIR})
set(Talib_LIBS Talib_SDK)

# 这里用到了一个CMake自带的函数，根据指定内容设置Talib_FOUND变量
include(FindPackageHandleStandardArgs)
# 必需的参数是Talib_LIBS，如果为空，则Talib_FOUND被设置为FALSE
# 并在使用find_package(Talib REQUIRED)时也会报错
find_package_handle_standard_args(
    Talib
    REQUIRED_VARS Talib_LIBS Talib_INCLUDE_DIRS
)
```

4. 编写CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 3.28)
project(TalibDemo)

set(CMAKE_CXX_STANDARD 17)

# 让CMake工程能够找到FindTalib.cmake
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
# 也可以将FindTalib.cmake放到ta-lib目录下，让CMake工程去这个目录下找
# list(APPEND CMAKE_MODULE_PATH "${THIRD_PARTY_DIR}/ta-lib")

# 如果将FindTalib.cmake放到ta-lib目录下，就可以不设置Talib_ROOT
set(Talib_ROOT ${THIRD_PARTY_DIR}/ta-lib)
find_package(Talib MODULE REQUIRED)

add_executable(TalibDemo main.cpp)

target_link_libraries(TalibDemo PRIVATE ${Talib_LIBS})

# 复制DLL文件
add_custom_command(TARGET TalibDemo POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
        ${Talib_DYNAMIC_LIB}
        $<TARGET_FILE_DIR:TalibDemo>)
```

5. 编写测试程序代码

```cpp
#include <ta_libc.h>
#include <vector>

int main() {
    TA_Initialize();

    std::vector<double> prices = {1,2,3,4,5};
    std::vector<double> out(prices.size());
    int outBeg, outNbElement;
    TA_RetCode ret = TA_SMA(0, prices.size()-1, prices.data(), 3, &outBeg, &outNbElement, out.data());
    if (ret == TA_SUCCESS) {
        for (int i = 0; i < outNbElement; ++i) {
            printf("Day %d = %f\n", outBeg+i, out[i]);
        }
    }
    TA_Shutdown();
    return 0;
}
```

## Ubuntu

1. 下载[ta-lib-0.6.4-src.tar.gz](https://github.com/ta-lib/ta-lib/releases)并解压：

```
tar -xzf ta-lib-0.6.4-src.tar.gz
cd ta-lib-0.6.4
```

2. 编译并安装

```
./configure --prefix=/path/to/third_party/ta-lib
make
sudo make install
```

卸载命令（会删除安装目录的头文件和库文件）：

```
sudo make uninstall
```

3. 编写CMakeLists.txt

```
cmake_minimum_required(VERSION 3.31)
project(TalibDemo)

set(CMAKE_CXX_STANDARD 17)

set(ENV{PKG_CONFIG_PATH} "${THIRD_PARTY_DIR}/ta-lib/lib/pkgconfig:$ENV{PKG_CONFIG_PATH}")

find_package(PkgConfig REQUIRED)
if(PKG_CONFIG_FOUND)
    pkg_check_modules(Talib REQUIRED IMPORTED_TARGET ta-lib)
endif()

add_executable(TalibDemo main.cpp)

target_link_libraries(TalibDemo PRIVATE PkgConfig::Talib)
```

#### 参考资料

- [【08】find_package 详解](https://www.cccolt.top/tutorial/cmake/08.html)
- [TA-Lib Install](https://ta-lib.org/install/)
