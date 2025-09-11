# Pybind11源码编译安装教程

## Windows下安装

1. 下载[pybind11-2.12.1.zip](https://github.com/pybind/pybind11/releases)源码并解压。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/pybind11-2.12.1
- Where to build the binaries: F:/third_party_build/pybind11-2.12.1/build
- CMAKE_INSTALL_PREFIX: F:/third_party/pybind11

3. 用Visual Studio打开build目录下生成的pybind11.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

## Linux下安装

1. 进入pybind11的源代码目录执行下面命令：

```
ccmake -S . -B ./build_linux -G "Unix Makefiles"
```

2. 输入c，修改完配置，再输入g：

- CMAKE_INSTALL_PREFIX: /path/to/third_party_linux/pybind11

3. 编译并安装：

```
cmake --build ./build_linux
cmake --install ./build_linux
```

## python调用C++

1. 编写CMakeLists.txt

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR})

# pybind11
#set(Python_ROOT_DIR "path to python document directory")
find_package(Python COMPONENTS Interpreter Development REQUIRED)
find_package(pybind11 CONFIG REQUIRED)
message(STATUS "Using pybind11 ${pybind11_VERSION}")

pybind11_add_module(cpp_module call_cpp.cpp)
```

2. 编写call_cpp.cpp代码：

```cpp
#include <pybind11/pybind11.h>

int add(int i, int j) {
    return i + j;
}

PYBIND11_MODULE(cpp_module, m) {
    m.doc() = "pybind11 example plugin";

    m.def("add", &add, "A function that adds two numbers");
}
```

3. 编写python测试代码：

```python
import sys
sys.path.append("./build")
import cpp_module

res = cpp_module.add(1, 2)
print(res)
```

## C++调用python

1. 编写CMakeLists.txt

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR})

# pybind11
find_package(pybind11 CONFIG REQUIRED)
message(STATUS "Using pybind11 ${pybind11_VERSION}")

add_executable(${PROJECT_NAME} call_python.cpp)

target_link_libraries(${PROJECT_NAME} PRIVATE pybind11::embed)
```

2. 编写script/call_python.cpp代码：

```cpp
#include <pybind11/embed.h>
namespace py = pybind11;

int main() {
    py::scoped_interpreter guard{};

    // add venv lib path
    py::module_::import("site").attr("addsitedir")("F:/venv/dev/Lib/site-packages");
    py::module_ sys = py::module_::import("sys");
    sys.attr("path").attr("append")("../script");

    py::print(sys.attr("path"));

    py::object module = py::module_::import("python_module");
    py::object result = module.attr("get_version")("hello");
    const std::string &version = result.cast<std::string>();
    py::print(version);
}
```

3. 编写python_module.py：

```python
import numpy as np

def get_version(s):
    return s + ":" + np.__version__
```

#### Linux中的问题

```
[cmake] CMake Error in CMakeLists.txt:
[cmake]   Imported target "pybind11::module" includes non-existent path
[cmake] 
[cmake]     "/usr/include/python3.12"
[cmake] 
[cmake]   in its INTERFACE_INCLUDE_DIRECTORIES.  Possible reasons include:
[cmake] 
[cmake]   * The path was deleted, renamed, or moved to another location.
[cmake] 
[cmake]   * An install or uninstall procedure did not complete successfully.
[cmake] 
[cmake]   * The installation package was faulty and references files it does not
[cmake]   provide.
```

解决方法：安装pip

```bash
sudo apt install python3-pip
```

#### 参考资料

- [pybind11 documentation](https://pybind11.readthedocs.io/en/stable/index.html)
- [https://stackoverflow.com/questions/70290168/using-pybind11-with-a-virtual-environment-created-at-runtime](https://stackoverflow.com/questions/70290168/using-pybind11-with-a-virtual-environment-created-at-runtime)
- [Embedding python with pybind11. Virtual environment doesn't work](https://stackoverflow.com/questions/56904149/embedding-python-with-pybind11-virtual-environment-doesnt-work)
