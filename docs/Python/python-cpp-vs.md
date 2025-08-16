# Visual Studio混合调试Python和C++

## 一、单独调试C++项目

### 创建C++项目

1. 创建一个C++的空项目，右键项目，选择properties，修改如下配置：

- 【General】->【Target Name】：example
- 【General】->【Configuration Type】：Dynamic Library (.dll)
- 【Advanced】->【Target File Extension】：.pyd
- 【C/C++】->【General】->【Additional Include Directories】：D:\Python\Python311\include;D:\third_party\pybind11\include
- 【Linker】->【General】->【Additional Library Directories】：D:\Python\Python311\libs

其他配置可参考官方文档的[配置项目属性](https://learn.microsoft.com/zh-cn/visualstudio/python/working-with-c-cpp-python-in-visual-studio?view=vs-2022#create-the-python-application)

2. 编写module.cpp内容如下：

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

### 在C++项目中启用调试

1. 在C++项目根目录下新建一个python脚本demo.py，内容如下：

```python
import sys
sys.path.append('./x64/Debug')
import example

res = example.add(1, 2)
print(res)
```

2. 右键C++项目，点击【Properties】->【Configuration Properties】->【Debugging】，进行如下修改：

- Debugger to launch: Python/Native Debugging
- Command: D:\Python\Python311\python.exe
- Command Arguments: .\demo.py

3. 最后在Python和C++源文件中分别打上断点，编译然后调试运行程序。

## 二、调试Python/C++混合项目

### 创建Python项目

1. 安装Visual Studio的python开发环境时需要勾选下面的选项：

- Python 3 64位(3.9.13)
- Python 本机开发工具

2. 创建一个Python Application项目，编写test.py：

```python
import example

res = example.add(1, 2)
print(res)
```

3. 右键解决方案，依次点击【Add】->【Existing Project】，然后选择上面创建的C++项目。

4. 右键Python项目，依次点击【Add】->【Reference】，勾选C++的项目名称，然后点击【OK】。

5. 右键解决方案，点击【Properties】，将【Configure Startup Projects】->【Single startup project】选择为Python项目，这样直接运行python程序就可以调用C++的动态库了。

## 三、在Python项目中启用调试

1. 如果使用Visual Studio安装的python就可以跳过这一步，否则在安装python环境时需要勾选下面两个选项，以安装Python解释器的调试符号：

- Download debugging symbols
- Download debug binaries (requires VS 2017 or later)

打开【Tools】-【Options】，找到【Debugging】-【Symbols】，在【Symbol file (.pdb) locations】中添加python.pdb文件所在的位置，如D:\Python\Python311

注：不推荐下载Python解释器的调试符号，这会引起pybind11链接错误LNK1104: cannot open file 'python311.lib'，见[[BUG]: Windows Python with Debug Libraries Installed causes linker error](https://github.com/pybind/pybind11/issues/3403#issuecomment-951485263)

2. 右键Python项目，点击【Properties】->【Debug】，勾选【Enable native code debugging】。

可选设置：【Properties】->【Debug】->【Intepreter Arguements】设置为-i可以让程序运行完不立即退出，而是停在python交互界面。

注：在Python项目中启用调试时，使用Visual Studio安装的python可以调试成功，但是使用自己安装的python环境调试时，import example出现ModuleNotFoundError，目前没有找到问题在哪。

#### 参考资料

- [在Visual Studio中同时调试Python和C++](https://learn.microsoft.com/zh-cn/visualstudio/python/debugging-mixed-mode-c-cpp-python-in-visual-studio?view=vs-2022)
