# vcpkg教程

## 安装vcpkg

1. 克隆存储库

```
git clone https://github.com/microsoft/vcpkg.git
```

2. 运行启动脚本

```
cd vcpkg
bootstrap-vcpkg.bat
```

3. 设置环境变量

```
set VCPKG_ROOT="C:\path\to\vcpkg"
set PATH=%VCPKG_ROOT%;%PATH%
```

## 设置Visual Studio项目

1. 使用VS创建CMake项目，命名为helloworld。

2. 在该项目目录下打开Developer Command Prompt：

```
# 生成vcpkg.json和vcpkg-configuration.json
vcpkg new --application
# 添加fmt包作为依赖项
vcpkg add port fmt
# 安装依赖项，项目下生成vcpkg_installed目录
vcpkg install
# 安装依赖项时指定triplet
vcpkg install --triplet x64-mingw-dynamic
# 查看vcpkg支持的triplet
vcpkg help triplet
```

3. 如需指定包的版本，可以如下修改vcpkg.json：

- 添加最低版本约束

```
{
  "dependencies": [
    {
        "name": "fmt",
        "version>=": "10.1.1"
    }
  ]
}
```

- 强制采用特定版本

```
{
  "dependencies": [
    "fmt"
  ],
  "overrides": [
    {
      "name": "fmt",
      "version": "10.1.1"
    }
  ]
}
```

## 设置CMake项目

1. 将CMakePresets.json文件重命名为CMakeUserPresets.json，并将如下<VCPKG_ROOT>替换为至vcpkg目录的路径：

```
{
    "version": 3,
    "configurePresets": [
        {
            "name": "default",
            "cacheVariables": {
                "CMAKE_TOOLCHAIN_FILE": "<VCPKG_ROOT>/scripts/buildsystems/vcpkg.cmake"
            }
        }
    ]
}
```

2. 编辑CMakeLists.txt文件：

```
cmake_minimum_required(VERSION 3.10)

project(HelloWorld)

find_package(fmt CONFIG REQUIRED)

add_executable(HelloWorld helloworld.cpp)

target_link_libraries(HelloWorld PRIVATE fmt::fmt)
```

3. 修改helloworld.cpp文件

```cpp
#include <fmt/core.h>

int main()
{
    fmt::print("fmt version is {}\n", FMT_VERSION);
    return 0;
}
```

## 常用命令

### git查询baseline

进入vcpkg安装目录，使用Git查看baseline：

```
# 获取vcpkg库最新的commit id（即baseline）
git rev-parse HEAD
# PowerShell查看依赖项版本
git show 3426db05b996481ca31e95fff3734cf23e0f51bc:versions/baseline.json | Select-String -Pattern '"zlib"|"fmt"' -Context 0,3
# bash查看依赖项版本
git show 3426db05b996481ca31e95fff3734cf23e0f51bc:versions/baseline.json | egrep -A 3 -e '"zlib"|"fmt"'
```

查找某个库所有版本对应的baseline，将<port-name>替换为要查看的库名称：

```
git log -- ports/<port-name>
```

如查看boost库：

```
git log --format="%H %s" -- ports/boost
git log --format="%H %cd %s" --date=short -- ports/boost
```

### baseline

```
# 添加初始builtin-baseline
vcpkg x-update-baseline --add-initial-baseline
# 更新baseline
vcpkg x-update-baseline
```

### install和remove

```
# 安装boost
vcpkg install boost
# 协助boost
vcpkg remove --recurse boost-uninstall
```

#### 参考资料

- [教程：在 Visual Studio 中使用 CMake 安装和使用包](https://learn.microsoft.com/zh-cn/vcpkg/get_started/get-started-vs?pivots=shell-cmd)
- [教程：安装特定版本的包](https://learn.microsoft.com/zh-cn/vcpkg/consume/lock-package-versions?tabs=inspect-bash)
- [git log format](https://www.cnblogs.com/ckAng/p/11205055.html)