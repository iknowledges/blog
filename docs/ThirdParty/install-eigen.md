# Eigen源码编译安装教程

## Windows下安装

1. 下载[eigen-3.4.0.zip](https://gitlab.com/libeigen/eigen/-/releases)源码并解压。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/eigen-3.4.0
- Where to build the binaries: F:/third_party_build/eigen-3.4.0/build
- CMAKE_INSTALL_PREFIX: F:/third_party/Eigen3

3. 用Visual Studio打开build目录下生成的Eigen3.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

## CMakeList.txt中使用

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR})

# Eigen3
find_package(Eigen3 CONFIG REQUIRED)
message(STATUS "Using Eigen3 ${Eigen3_VERSION}")

target_link_libraries(${PROJECT_NAME} PRIVATE Eigen3::Eigen)
```

编写测试代码main.cpp

```cpp
#include <iostream>
#include <Eigen/Dense>

using Eigen::MatrixXd;

int main() {
    MatrixXd m(2, 2);
    m(0, 0) = 3;
    m(1, 0) = 2.5;
    m(0, 1) = -1;
    m(1, 1) = m(1, 0) + m(0, 1);
    std::cout << m << std::endl;
    return 0;
}
```

#### 参考资料

- [Eigen官网](https://eigen.tuxfamily.org/)
