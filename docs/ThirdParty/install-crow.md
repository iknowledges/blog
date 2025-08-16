# CMake配置Crow

1. 下载[Crow-1.1.1.zip](https://github.com/CrowCpp/Crow/releases/tag/v1.1.0)，并解压到项目的3rdpart文件夹下。

2. 配置CMakeLists.txt：

```
cmake_minimum_required(VERSION 3.28)
project(CrowDemo)

set(CMAKE_CXX_STANDARD 17)

# Crow
list(APPEND CMAKE_PREFIX_PATH "${CMAKE_SOURCE_DIR}/3rdpart/Crow-1.1.1")
find_package(Crow REQUIRED CONFIG)

add_executable(CrowDemo main.cpp)

if(WIN32)
    target_link_libraries(CrowDemo PUBLIC wsock32 ws2_32 Crow::Crow)
else()
    target_link_libraries(CrowDemo PUBLIC Crow::Crow)
endif()
```

3. 编写示例代码main.cpp：

```cpp
#include "crow.h"

int main() {
    crow::SimpleApp app;

    CROW_ROUTE(app, "/")([](){
        return "Hello world";
    });

    app.port(18080).run();
}
```

#### 参考资料

- [Crow - Getting Started](https://crowcpp.org/master/getting_started/setup/linux/)
