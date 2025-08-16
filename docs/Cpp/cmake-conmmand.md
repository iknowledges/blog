# CMake命令教程

## CMake命令实现程序的分步生成

首先生成CMake项目，并查看所有目标：

```
# Linux
cmake -S . -B build
# Windows 必须在VS的控制台执行
cmake -S . -B build -G "NMake Makefiles"
# 查看所有目标
cmake --buil ./build --target help
```

然后分步实现编译过程：

1. 预处理

```
cmake --build ./build --target my_project.i
```

2. 编译

```
cmake --build ./build --target my_project.s
```

3. 汇编

```
cmake --build ./build --target my_project.o
```

4. 链接

```
cmake --build ./build
```

#### 其他命令

```
# 清理项目
cmake --build ./build --target clean
```

## CMake调试打印生成的指令

- 方法一：在CMakeLists.txt中设置变量CMAKE_VERBOSE_MAKEFILE

```
# 显示详细的生成日志，默认是OFF
set(CMAKE_VERBOSE_MAKEFILE ON)
```

- 方法二：在编译命令中添加--verbose或-v选项

```
cmake --build ./build -v
```

## ccmake使用

```
ccmake -S . -B build
```
