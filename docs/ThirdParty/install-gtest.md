# Google Test源码编译安装教程

## Windows下安装

1. 下载[googletest-1.16.0.tar.gz](https://github.com/google/googletest/releases)源码并解压。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/googletest-1.16.0
- Where to build the binaries: F:/third_party_build/googletest-1.16.0/build
- CMAKE_INSTALL_PREFIX: F:/third_party/GTest

3. 用Visual Studio打开build目录下生成的googletest-distribution.sln工程，找到gmock、gmock_main、gtest和gtest_main这几个项目，右键项目点击属性，依次找到【C/C++】->【代码生成】->【运行库】，将`/MTd`改成`/MDd`，因为VS默认项目设置是“多线程调试DLL(/MDd)”，而GTest库的运行库设置成了“多线程调试(/MTd)”，这样两者不一致会导致编译时很多链接错误。最后右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

## CMakeList.txt中使用GTest

参考源代码目录下的googletest/README.md文件：

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR})

# gtest
find_package(GTest CONFIG REQUIRED)
message(STATUS "Using GTest ${GTest_VERSION}")

enable_testing()

add_executable(hello_test hello_test.cpp)

target_link_libraries(hello_test PRIVATE GTest::gtest_main)

add_test(NAME hello COMMAND hello_test)
```

编写测试代码hello_test.cpp：

```cpp
#include <gtest/gtest.h>

TEST(HelloTest, BasicAssertions) {
    EXPECT_STRNE("hello", "world");

    EXPECT_EQ(7 * 6, 42);
}
```

#### 参考资料

- [Quickstart: Building with CMake](https://google.github.io/googletest/quickstart-cmake.html)
- [开始尝试google test单元测试工具（又是MTd/MDd搞的鬼！）附带VC运行库详解](https://www.cnblogs.com/tobyforever/archive/2009/05/01/1447473.html)
