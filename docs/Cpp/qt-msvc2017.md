# Qt安装MSVC2017编译器

1. 首先安装好Qt和Visual Studio 2019，然后再Visual Studio Installer选择修改，安装MSVC2017对应的组件。

![qt-msvc2017-1](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-msvc2017-1.png)

2. 下载并安装最新版[Windows 10 SDK](https://developer.microsoft.com/zh-cn/windows/downloads/sdk-archive/)

![qt-msvc2017-2](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-msvc2017-2.png)

只要选择`Debugging Tools for Windows`，然后找到下载的Installers目录，依次安装下载的安装包。

![qt-msvc2017-3](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-msvc2017-3.png)

3. 打开Qt Creator工具，选择【工具】->【选项】->【编译器】->【添加】->【MSVC】->【C++】，然后如下图配置编译器：

![qt-msvc2017-4](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-msvc2017-4.png)

![qt-msvc2017-5](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-msvc2017-5.png)

编译器可以被自动探测到：

![qt-msvc2017-6](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-msvc2017-6.png)

4. 最后如下配置构建套件：

![qt-msvc2017-7](https://gitlab.com/iknowledge/BlogImage/-/raw/main/Qt/qt-msvc2017-7.png)