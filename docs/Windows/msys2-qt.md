# MSYS2安装Qt开发环境

## MSYS2

使用pacman安装qt5以及编译工具，第三方插件可以不安装：

```bash
pacman -S mingw-w64-ucrt-x86_64-qt5
pacman -S mingw-w64-ucrt-x86_64-gcc
pacman -S mingw-w64-ucrt-x86_64-gdb
pacman -S mingw-w64-ucrt-x86_64-make
pacman -S mingw-w64-ucrt-x86_64-pkgconf
pacman -S mingw-w64-ucrt-x86_64-cmake
# 第三方插件
pacman -S mingw-w64-ucrt-x86_64-poppler-qt5
```

## Qt Creator

1. 到[官网地址](https://download.qt.io/official_releases/qtcreator/)下载`qt-creator-opensource-windows-x86_64-13.0.0.exe`

2. 打开【编辑】->【Preferences】->【CMake】->【Tools】，添加CMake

![cmake](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/msys2-qtcreator-cmake.png)

3. 打开【编辑】->【Preferences】->【构建套件】->【编译器】

- 添加gcc

![gcc](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/msys2-qtcreator-gcc.png)

- 添加g++

![g++](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/msys2-qtcreator-gpp.png)

4. 打开【编辑】->【Preferences】->【构建套件】->【Debuggers】，添加gdb

![gdb](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/msys2-qtcreator-gdb.png)

5. 打开【编辑】->【Preferences】->【构建套件】->【Qt版本】，添加Qt

![qmake](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/msys2-qtcreator-qmake.png)

6. 打开【编辑】->【Preferences】->【构建套件】->【Kit】，进行如下设置：

![lit](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/msys2-qtcreator-kit.png)
