# Python和C++混合调试方案

1. 编写C++代码main.cpp：

```cpp
#include <pybind11/pybind11.h>

int add(int i, int j) {
    return i + j;
}

namespace py = pybind11;

PYBIND11_MODULE(example, m) {
    m.def("add", &add, R"pbdoc(
        Add two numbers

        Some other explanation about the add function.
    )pbdoc");
}
```

2. 编写Python代码test.py：

```python
import os
print(os.getpid())
import example

res = example.add(1, 2)
print(res)
```

3. 安装好VSCode的C++和Python插件后，创建launch.json：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python Debugger: Current File",
            "type": "debugpy",
            "request": "launch",
            "program": "${workspaceFolder}/test.py",
            "console": "integratedTerminal"
        },
        {
            "name": "(gdb) Attach",
            "type": "cppdbg",
            "request": "attach",
            "program": "/usr/bin/python3.13",
            "processId":"${command:pickProcess}",
            "MIMode": "gdb",
            "miDebuggerPath": "/usr/bin/gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        },
    ]
}
```

4. 编译调试用的库文件：

```
g++ -g -Wall -shared -std=c++14 -fPIC $(python3.13 -m pybind11 --includes) main.cpp -o example$(python3.13-config --extension-suffix)
```

也可以用CMake进行编译，CMakeLists.txt如下：

```
cmake_minimum_required(VERSION 3.20)
project(example LANGUAGES CXX)

set(CMAKE_BUILD_TYPE Debug)

set(PYBIND11_FINDPYTHON ON)
find_package(pybind11 CONFIG REQUIRED)

pybind11_add_module(example main.cpp)
```

5. 在源代码中打好断点，先启动【Python Debugger】，再启动【(gdb) Attach】，输入python程序的pid，就可以开始调试C++代码了。

6. 以上调试过程是在Linux环境中进行的，由于Windows上没有gdb和gcc，所以要使用Visual Studio编译C++代码，并修改launch.json配置如下：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python Debugger: Current File",
            "type": "debugpy",
            "request": "launch",
            "program": "${workspaceFolder}/test.py",
            "console": "integratedTerminal"
        },
        {
            "name": "(Windows) Attach",
            "type": "cppvsdbg",
            "request": "attach",
            "processId": "${command:pickProcess}"
        }
    ]
}
```

## VSCode插件Python C++ Debugger

混合调试插件[Python C++ Debugger](https://marketplace.visualstudio.com/items?itemName=benjamin-simmonds.pythoncpp-debug)配置如下：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python C++ Debug",
            "type": "pythoncpp",
            "request": "launch",
            "pythonLaunchName": "Python: Current File",
            "cppAttachName": "(Windows) Attach",
        },
        {
            "name": "(Windows) Attach",
            "type": "cppvsdbg",
            "request": "attach",
            "processId": ""
        },
        {
            "name": "Python: Current File",
            "type": "debugpy",
            "request": "launch",
            "program": "${workspaceFolder}/test.py",
            "console": "integratedTerminal"
        }
    ]
}
```

## 使用Clion调试C++代码

配置CMake Application如下：

- Target: example
- Executable: /path/to/python.exe
- Program arguments: ./test.py
- Working directory: /path/to/project root/
- Environment variables: PYTHONPATH=./cmake-build-debug;PYTHONUNBUFFERED=1

注：使用python虚拟环境进行调试时，必须用管理员身份执行`python -m venv dev --symlinks`来创建虚拟环境。

#### 参考资料

- [Python,cpp混合编程调试方案](https://www.bilibili.com/video/BV1wV4y1p7Ak)
- [Debug Pybind11 C++/Python mixture project with CLion](https://rancheng.github.io/Debug-pybind-within-CLion/)
- [Debug Python extensions](https://www.jetbrains.com/help/clion/debugging-python-extensions.html#debug-custom-py)
