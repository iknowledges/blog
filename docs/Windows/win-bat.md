# 批处理文件用法

## if语句

指令格式为：if 条件表达式 (...)

1. if和else if均在一行

```
@echo off
set inputValue=15
if %inputValue% gtr 100 (echo %inputValue%大于100) else if %inputValue% gtr 10 (echo %inputValue%大于10) else (echo %inputValue%小于或等于10) 
pause
```

- @echo off: 表示关闭所有命令的回显。

2. if和else if在多行

```
@echo off

set inputValue=23

if %inputValue% gtr 100 (
	echo 大于100
) else if %inputValue% gtr 10 (
	echo 大于10
) else if %inputValue% gtr 0 (
	echo 大于0
)
pause
```

3. if和else if在多行

```
@echo off

set inputValue=23

if %inputValue% gtr 100 (
	echo 大于100
)^
else if %inputValue% gtr 10 (
	echo 大于10
)^
else if %inputValue% gtr 0 (
	echo 大于0
)
pause
```

## for循环

```
在cmd窗口中：for %i in (command1) do command2
在批处理文件中：for %%i in (command1) do command2
```

- %i指变量，可以用任意字母代替，区分大小写。

如结束mysqld进程：

```
for /f %%i in ('tasklist /nh') do if "%%i"=="mysqld.exe" (taskkill /f /c /im mysqld.exe)
```

## %~dp0的含义

- ~：是扩展的意思，变量扩充，相当于把一个相对路径转换成绝对路径。
- %0：指批处理文件自身。（绝对路径 "D:\test\bat\test.bat"，注意有双引号）
- %~d0：是指批处理所在的盘符，其中d代表drive。(扩充到盘符D:)
- %~p0：是指批处理所在的目录，其中p代表path。(扩充到路径\test\bat\）
- %~dp0：是批处理所在的盘符加路径。（扩充到盘符和路径：D:\test\bat\）

## >重定向操作符

#### 2>&1

2>&1的含义是将句柄2（即STDERR）重定向到句柄1（即STDOUT）。

- 0表示：标准输入。
- 1表示：标准输出。
- 2表示：标准错误。
- &表示引用，&1就是对标准输出的引用。

#### nul用法

- 2>nul：屏蔽操作失败显示的信息，如果成功依旧显示。
- >nul（即1>nul）：屏蔽操作成功显示的信息，但是出错还是会显示。

## 获取管理员权限

```
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"
if '%errorlevel%' NEQ '0' ( goto UACPrompt ) else ( goto gotAdmin )
:UACPrompt
echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"
echo UAC.ShellExecute "%~s0", "", "", "runas", 1 >> "%temp%\getadmin.vbs"
"%temp%\getadmin.vbs"
exit /B
:gotAdmin
if exist "%temp%\getadmin.vbs" ( del "%temp%\getadmin.vbs" )

::下面写你自己的代码
```

#### 参考资料

- [Windows批处理(bat) if条件判断语句使用教程](https://blog.csdn.net/m0_56208280/article/details/129129468)
- [Windows批处理(cmd/bat)常用命令教程](https://blog.csdn.net/sirobot/article/details/120367532)
- [windows脚本bat五种获取管理员权限的方法](https://blog.csdn.net/lionking1990/article/details/122270881)
