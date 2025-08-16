# GDB调试教程

## 启动GDB的命令

```
# 调试可执行程序
gdb <program>
# 调试core文件(core文件是程序非法执行后core dump后产生的文件)
gdb <program> <core dump file>
# 调试运行中的程序
gdb <program> <PID>
gdb -p <PID>
```

## GDB交互命令

### 运行相关命令

- run（简写r）：其作用是运行程序，当遇到断点后，程序会在断点处停止运行，等待用户输入下一步的命令，run后可以带一些命令，比如我们要调试main程序，我们第一步是gdb main，进入gdb后，可以输入run log.conf去运行程序。

- continue（简写c）：继续执行，到下一个断点处（或运行结束）。

- next（简写n）：单步跟踪程序，当遇到函数调用时，也不进入此函数体；此命令同step的主要区别是，step遇到用户自定义的函数，将进入函数中去运行，而next则直接调用函数，不会进入到函数体内。

- step（简写s）：单步调试如果有函数调用，则进入函数；与命令n不同，n是不进入调用的函数的。

- until：当你厌倦了在一个循环体内单步跟踪时，这个命令可以运行程序直到退出循环体。

- until+行号：运行至某行，不仅仅用来跳出循环。

- finish：运行程序，直到当前函数完成返回，并打印函数返回时的堆栈地址和返回值及参数值等信息。

- call 函数(参数)：调用程序中可见的函数，并传递“参数”，如：call gdb_test(55)

- quit：简记为q，退出gdb。

### 设置断点

- break n（简写b n）：在第n行处设置断点（可以带上代码路径和行号：b main.cpp:578）。

- break fn1 if a＞b：条件断点设置。

- break func：在函数func()的入口处设置断点，如：break gdb_test。

- delete 断点号n：删除第n个断点。

- disable 断点号n：暂停第n个断点。

- enable 断点号n：开启第n个断点。

- clear 行号n：清除第n行的断点。

- info breakpoints（简写info b）：显示当前程序的断点设置情况。

- delete breakpoints：清除所有断点。

- thread [id]：切换到具体的线程id，一般切换到具体的线程后再执行bt等操作。

### 查看源代码

- list（简写l）：其作用就是列出程序的源代码，默认每次显示10行。

- list：不带参数，将接着上一次list命令的，输出下边的内容。

- list 行号：将显示当前文件以“行号”为中心的前后10行代码，如：list 12

- list 函数名：将显示“函数名”所在函数的源代码，如：list main

### 打印表达式

- print 表达式（简记为p）：其中“表达式”可以是任何当前正在被测试程序的有效表达式，包括数字，变量甚至是函数调用。

- print a：将显示变量a的值。

- print gdb_test(22)：将以整数22作为参数调用gdb_test()函数。

- print gdb_test(a)：将以变量a作为参数调用gdb_test()函数。

- display 表达式：在单步运行时将非常有用，使用display命令设置一个表达式后，它将在每次单步进行指令后，紧接着输出被设置的表达式及值。如：display a。

- watch 表达式：设置一个监视点，一旦被监视的“表达式”的值改变，gdb将强行终止正在被调试的程序。如：watch a。

- whatis：查询变量或函数。

- info function：查询函数。

- 扩展info locals：显示当前堆栈页的所有变量。

### 表达式格式化输出

- set print address [on/off]：打开地址输出，当程序显示函数信息时，GDB会显出函数的参数地址。系统默认为打开的。

- set print array [on/off]：打开数组显示，打开后当数组显示时，每个元素占一行，如果不打开的话，每个元素则以逗号分隔。这个选项默认是关闭的。

- set print elements：这个选项主要是设置数组的，如果你的数组太大了，那么就可以指定一个来指定数据显示的最大长度，当到达这个长度时，GDB就不再往下显示了。如果设置为0，则表示不限制。

- set print null-stop：当显示字符串时，遇到结束符则停止显示。这个选项默认为off。

- set print pretty [on/off]：当GDB格式化输出结构体。

### 查询运行信息

- where/bt：当前运行的堆栈列表。

- bt backtrace：显示当前调用堆栈。

- up/down：改变堆栈显示的深度。

- set args 参数：指定运行时的参数。

- show args：查看设置好的参数。

- info program：来查看程序的是否在运行，进程号，被暂停的原因。

- info threads: 查看所有线程。

### 分割窗口

- layout：用于分割窗口，可以一边查看代码，一边测试：

- layout src：显示源代码窗口

- layout asm：显示反汇编窗口

- layout regs：显示源代码/反汇编和CPU寄存器窗口

- layout split：显示源代码和反汇编窗口

- Ctrl + L：刷新窗口

## GDB TUI——在GDB中显示程序源码

官方文档：http://sourceware.org/gdb/onlinedocs/gdb/TUI.html

### 开启GDB TUI模式

- 方法一：使用gdbtui命令或者gdb-tui命令开启一个调试。

```
gdbtui -q 需要调试的程序名
```

- 方法二：直接使用GDB调试代码，再使用快捷键Ctrl + x + a进行切换。

## GDB调试coredump

- ulimit

```bash
# 查看core文件配置
ulimit -a
# 设置生成core文件大小
ulimit -c unlimited
```

- 定义core文件生成路径

```bash
# 查看core文件生成路径
cat /proc/sys/kernel/core_pattern
# 设置core文件生成路径
echo /home/user/core_log/core_%e_%t_%p > /proc/sys/kernel/core_pattern
```

## cmake编译支持GDB配置

```
SET(CMAKE_BUILD_TYPE "Debug")
SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g3 -ggdb")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")
```

gcc生成调试信息的一些选项：

- -Wall：生成所有警告信息。

- -O0、-O1、-O2、-O3：编译器的优化选项的4个级别，-O0表示没有优化, -O1为默认值，-O3优化级别最高。

- -g：该选项可以利用操作系统的“原生格式（native format）”生成调试信息。GDB可以直接利用这个信息，其它调试器也可以使用这个调试信息。

- -ggdb：使GCC为GDB生成专用的更为丰富的调试信息，但是，此时就不能用其他的调试器来进行调试了(如ddx)。

-g是分级别的：

- -g2：这是默认的级别，此时产生的调试信息包括扩展的符号表、行号、局部或外部变量信息。

- -g3：包含级别2中的所有调试信息，以及源代码中定义的宏，以及C++中的内联函数（inline）。

- -g1：级别1不包含局部变量和与行号有关的调试信息，因此只能够用于回溯跟踪和堆栈转储。回溯跟踪指的是监视程序在运行过程中的函数调用历史，堆栈转储则是一种以原始的十六进制格式保存程序执行环境的方法，两者都是经常用到的调试手段。

## gdb调试tritonserver案例

### 正常启动

1. 监听进程：

```
# 每隔1秒监控进程的输出
watch -n 1 "ps -elf"
```

2. 正常启动程序

```
./tritonserver --model-repository /workspace/tritonserver/model-repository --backend-directory /build/tritonserver/install/backends --http-port 10000 --log-verbose 1 --log-info true --log-warning true --log-error true --log-format ISO8601 --log-file ./triton.log --id tritonserver &
```

3. 关闭进程

```
kill -9 <父进程PID>
```

### 调试启动

```
# 启动gdb
gdb -tui ./tritonserver
# 启动程序
start --model-repository /workspace/tritonserver/model-repository --backend-directory /build/tritonserver/install/backends --http-port 10000 --log-verbose 1 --log-info true --log-warning true --log-error true --log-format ISO8601 --log-file ./triton.log --id tritonserver &
# 在487行打断点，如果在当前文件路径可以省略
b /workspace/tritonserver/src/main.cc:487
# 查看断点
info b
# 跳到断点
c
# 跳到函数内部
s
# 单步调试
n
# 打印变量addr
p addr
```

### 多进程调试

- 一个窗口监听进程

```
watch -n 1 "ps -elf"
```

- 一个窗口调试父进程

```
# 启动gdb
gdb -tui ./tritonserver
# 启动程序
start --model-repository /workspace/tritonserver/model-repository --backend-directory /build/tritonserver/install/backends --http-port 10000 --log-verbose 1 --log-info true --log-warning true --log-error true --log-format ISO8601 --log-file ./triton.log --id tritonserver &
# 打断点
break /tmp/tritonbuild/python/src/stub_launcher.cc:442
```

调试父进程的时候，观察进程监听窗口对应子进程的变化，找到子进程的PID，如74165

- 一个窗口调试子进程

```
# 启动gdb
gdb -tui
# 调试运行中的进程
attach 74165
# 打断点
break /tmp/tritonbuild/python/src/pb_stub.cc:529
```

## gdb调试技巧

- 打印变量

```
# 打印wstring
p str._M_data()
# 打印wchar_t
p (wchar_t) ch
```

#### 参考资料

- [gdb与coredump调试技巧](https://blog.csdn.net/u012173846/article/details/119654828)
- [gdb调试coredump(使用篇)](https://blog.csdn.net/qq_39759656/article/details/82858101)
- [GCC参数详解](https://www.runoob.com/w3cnote/gcc-parameter-detail.html)
