# VSCode配置MSYS2开发环境

## VSCode设置MSYS2为默认终端

在settings.json中加入如下内容：

```json
{
    "terminal.integrated.defaultProfile.windows": "MSYS2",
    "terminal.integrated.profiles.windows": {
        "MSYS2": {
            "path": [
                "D:\\msys64\\start\\msys.bat",
            ],
            "args": [],
            "icon": "terminal-cmd"
        }
    },
}
```

## VSCode配置C开发环境

1. MSYS2安装gcc和gdb

```
pacman -S ucrt64/mingw-w64-ucrt-x86_64-gcc
pacman -S ucrt64/mingw-w64-ucrt-x86_64-gdb
```

2. VSCode配置默认编译器，修改settings.json：

```
{
    "C_Cpp.default.compilerPath": "D:\\msys64\\ucrt64\\bin\\gcc.exe"
}
```

路径可以通过下面命令查找：

```
where gcc
```

3. VSCode设置调试环境，创建launch.json，选择【C++(GDB/LLDB)】,然后添加【(gdb) Launch】配置：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "(gdb) Launch",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/a.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${fileDirname}",
            "environment": [],
            "externalConsole": true,
            "MIMode": "gdb",
            "miDebuggerPath": "D:\\msys64\\ucrt64\\bin\\gdb.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                },
                {
                    "description": "Set Disassembly Flavor to Intel",
                    "text": "-gdb-set disassembly-flavor intel",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```

- program: 设置为启动文件的路径
- miDebuggerPath: 设置为gdb的路径
- externalConsole: 是否打开外部cmd窗口，默认为false，这里改为true

注意启动调试时，要用下面命令生成可调式的二进制文件：

```
gcc hello.c -g
```

#### 命令行中调试方法

1. 使用gdb调试hello.c

```
gcc hello.c -g
gdb a.exe
```

2. gdb命令：

```
# 在main函数打断点
b main
# 开始执行
r
# 单步调试
n
# 反汇编查看
disasemble
# 退出gdb
q
```

## VSCode配置CMake

1. MSYS2安装CMake：

```
pacman -S ucrt64/mingw-w64-ucrt-x86_64-cmake
```

2. VSCode修改settings.json：

```json
{
    "cmake.cmakePath": "D:\\Software\\msys64\\ucrt64\\bin\\cmake.exe"
}

```