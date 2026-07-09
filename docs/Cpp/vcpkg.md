# vcpkg教程

## 安装vcpkg

1. 克隆存储库

```
git clone https://github.com/microsoft/vcpkg.git
```

2. 运行启动脚本

```
cd vcpkg
.\bootstrap-vcpkg.bat -disableMetrics
```

- disableMetrics: 表示禁用数据采集

3. 在CMD中设置环境变量，或者设置成系统环境变量，那么就不用输入下面命令：

```
set VCPKG_ROOT="C:\path\to\vcpkg"
set PATH=%VCPKG_ROOT%;%PATH%
```

4. 测试是否安装成功：

```
# 查看版本
vcpkg version
# 查看所有指令
vcpkg help topics
```

## 开发中使用vcpkg

### 创建测试项目

1. 项目结构如下：

```
helloworld/
├── CMakeLists.txt
└── helloworld.cpp
```

2. 编写CMakeLists.txt文件：

```
cmake_minimum_required(VERSION 3.10)

project(HelloWorld)

find_package(fmt CONFIG REQUIRED)

add_executable(HelloWorld helloworld.cpp)

target_link_libraries(HelloWorld PRIVATE fmt::fmt)
```

3. 编写helloworld.cpp文件

```cpp
#include <fmt/core.h>

int main()
{
    fmt::print("fmt version is {}\n", FMT_VERSION);
    return 0;
}
```

### 使用命令行编译

1. 安装最新版本的依赖

```
vcpkg install fmt
```

2. 安装指定版本的依赖（可选）

```
# 生成vcpkg.json和vcpkg-configuration.json
vcpkg new --application
# 将fmt包作为依赖项添加到vcpkg.json，不会进行安装，cmake配置阶段会自动调用vcpkg install安装
vcpkg add port fmt
```

- 修改vcpkg.json添加最低版本约束

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

- 修改vcpkg.json强制采用特定版本

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

3. 开始编译

```
mkdir build
cmake -B build -DCMAKE_TOOLCHAIN_FILE=path/to/vcpkg/scripts/buildsystems/vcpkg.cmake
cmake --build build
```

### 使用VSCode编译

- 方法一：在项目的根目录添加`CMakePresets.json`文件：

```
{
    "version": 3,
    "configurePresets": [
        {
            "name": "default",
            "cacheVariables": {
                "CMAKE_TOOLCHAIN_FILE": "path/to/vcpkg/scripts/buildsystems/vcpkg.cmake"
            }
        }
    ]
}
```

- 方法二：修改项目根目录下的`.vscode/settings.json`文件：

```json
{
    "cmake.configureSettings": {
        "CMAKE_TOOLCHAIN_FILE": "path/to/vcpkg/scripts/buildsystems/vcpkg.cmake"
    }
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

### 其他

```
# 安装依赖项时指定triplet
vcpkg install --triplet x64-mingw-dynamic
# 查看vcpkg支持的triplet
vcpkg help triplet
# 列出已安装的包
vcpkg list
# 安装boost
vcpkg install boost
# 协助boost
vcpkg remove --recurse boost-uninstall
```

#### 参考资料

- [Tutorial: Install and use packages with CMake](https://learn.microsoft.com/en-us/vcpkg/get_started/get-started?pivots=shell-powershell)
- [Tutorial: Install a specific version of a package](https://learn.microsoft.com/en-us/vcpkg/consume/lock-package-versions?tabs=inspect-powershell)
- [git log format](https://www.cnblogs.com/ckAng/p/11205055.html)