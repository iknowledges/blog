# VSCode配置MinGW开发环境

1. 下载[x86_64-13.1.0-release-posix-seh-ucrt-rt_v11-rev1.7z](https://github.com/niXman/mingw-builds-binaries/releases)，解压后，将mingw64/bin加到系统环境变量。

2. 用vscode打开CMake项目，因为要使用MSYS2安装的第三方依赖，所以如下配置settings.json：

```json
{
    "cmake.cmakePath": "D:/msys64/ucrt64/bin/cmake.exe",
    "cmake.generator": "MinGW Makefiles",
    "cmake.configureSettings": {
        "CMAKE_C_COMPILER": "D:/msys64/ucrt64/bin/gcc.exe",
        "CMAKE_CXX_COMPILER": "D:/msys64/ucrt64/bin/g++.exe",
        "CMAKE_MAKE_PROGRAM": "D:/msys64/ucrt64/bin/mingw32-make.exe"
    }
}
```

3. Ctrl+Shift+P打开vscode命令窗口，选择CMake: Select a Kit，然后选择刚刚安装的MinGW编译器。

4. Ctrl+Shift+P打开vscode命令窗口，选择CMake: Configure，最后就可以编译执行程序了。

## 问题解决：

- 编译报错：too many sections ... File too big

在CMakeLists.txt中加入如下内容：

```
set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-O3")
```

#### 参考资源

- [Options That Control Optimization](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html#Optimize-Options)
