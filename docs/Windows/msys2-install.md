# MSYS2安装教程

## 安装

1. 下载[msys2-x86_64-20240113.exe](https://github.com/msys2/msys2-installer/releases)，双击进行安装。

2. 在安装目录下新建\start\msys.bat，写入如下命令：

```
@echo off
set MSYS2=D:\msys64
if "%1"=="" goto noarg
%MSYS2%\msys2_shell.cmd -ucrt64 -defterm -here -full-path -no-start -c "%*"
goto :eof
:noarg
%MSYS2%\msys2_shell.cmd -ucrt64 -defterm -here -full-path -no-start
```

然后将如下路径加入系统环境变量：

```
D:\msys64\start
```

这样就可以在cmd中输入msys命令打开窗口了。

#### 注意事项

- 不要将`D:\msys64\usr\bin`和`D:\Software\msys64\ucrt64\bin`加入环境变量，以免污染系统环境，造成不同环境冲突。

#### 参考资源

- [MSYS2](https://www.msys2.org/)
- [给萌新的C/C++环境搭建攻略（VSCode和MSYS2）](https://zhuanlan.zhihu.com/p/401188789)
