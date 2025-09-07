# Conan2使用教程

## 安装和卸载Conan

```bash
pip install conan
pip uninstall conan
rm -rf ~/.conan2/
```

## 开发流程

1. 创建一个CMake项目，目录结构如下：

```
.
├── CMakeLists.txt
├── conanfile.txt
└── src
    └── main.cpp
```

CMakeLists.txt内容如下：

```
cmake_minimum_required(VERSION 3.15)
project(conan-example)

find_package(fmt REQUIRED)

add_executable(${PROJECT_NAME} src/main.cpp)

target_link_libraries(${PROJECT_NAME} fmt::fmt)
```

conanfile.txt内容如下：

```
[requires]
fmt/11.0.2

[generators]
CMakeDeps
CMakeToolchain
```

测试代码main.cpp内容如下：

```cpp
#include <fmt/core.h>
#include <fmt/color.h>

int main(int argc, char const *argv[]) {
    fmt::print(fmt::fg(fmt::color::cyan) | fmt::emphasis::bold, "Hello Conan!\n");
    return 0;
}
```

2. 默认Release版本编译过程

```bash
# 检测当前操作系统环境，生成Conan配置文件，默认build_type为Release
conan profile detect
# 查看生成的配置文件default
cat ~/.conan2/profiles/default
# 安装依赖，--output-folder指定编译生成目录
conan install . --output-folder=cmake-build-release --build=missing
# 配置CMake并进行编译
cd cmake-build-release
cmake .. -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Release
cmake --build .
```

3. 使用Debug版本编译过程

```bash
# 生成build_type为Debug的配置文件
conan profile detect --name debug
# 修改生成的配置文件debug，将build_type改成Debug
vim ~/.conan2/profiles/debug
# 安装依赖
conan install . --output-folder=cmake-build-debug --build=missing --profile=debug
# 配置CMake并进行编译
cd cmake-build-debug
cmake .. -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Debug
cmake --build .
```

4. 使用VSCode编译配置，修改`.vscode/settings.json`如下：

```json
{
    "cmake.configureArgs": [
        "-DCMAKE_BUILD_TYPE=Debug",
        "-DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake"
    ]
}
```

#### layout的作用

layout可以设置编译生成的代码布局。cmake_layout是内置的cmake布局方式，并会生成CMakePresets.json文件。

1. 将上面项目的conanfile.txt修改为：

```
[requires]
fmt/11.0.2

[generators]
CMakeDeps
CMakeToolchain

[layout]
cmake_layout
```

2. 编译命令修改为：

```bash
conan profile detect
conan install . --output-folder=build --build=missing
# 查看可用的预设配置
cmake --list-presets
# 指定预设进行配置
cmake --preset=conan-release
# 指定预设进行编译
cmake --build --preset=conan-release
```

#### 通过proxy下载依赖

因为Conan底层使用requests进行网络请求，所以参考[requests官方文档](https://docs.python-requests.org/en/latest/user/advanced/#proxies)进行设置。

1. 安装支持socks协议的requests包

```
pip install 'requests[socks]'
```

2. 设置环境变量

```
export HTTP_PROXY="socks5://10.10.1.10:3434"
export HTTPS_PROXY="socks5://10.10.1.10:3434"
export ALL_PROXY="socks5://10.10.1.10:3434"
```

## 其他Conan命令

```bash
# 列出所有已安装的依赖
conan list '*'
# 删除指定依赖
conan remove boost/1.79.0
# 查看依赖的安装路径
conan cache path boost/1.79.0
# 从远程仓库搜索
conan search boost --remote=conancenter
# 查看远程仓库地址
conan remote list
# 检查当前目录下conanfile.txt中的依赖关系图
conan graph info .
```

#### 参考资料

- [使用 Conan 构建一个简单的 CMake 项目](https://docs.conan.org.cn/2/tutorial/consuming_packages/build_simple_cmake_project.html)
- [conan2.0 基础入门 (主流的C/C++包管理工具)](https://www.bilibili.com/video/BV18s421A7Jj)
- [理解 Conan 包布局](https://docs.conan.org.cn/2/tutorial/developing_packages/package_layout.html)
