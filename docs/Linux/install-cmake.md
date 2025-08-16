# CMake安装教程

## Ubuntu

### 方法1：预编译版安装

1. 在官网下载安装压缩包[cmake-4.0.3-linux-x86_64.tar.gz](https://cmake.org/download/)，并解压：

```
tar -zxvf cmake-4.0.3-linux-x86_64.tar.gz
```

2. 修改`~/.bashrc`配置环境变量：

```
# CMake
export CMAKE_HOME=/path/to/cmake-4.0.3-linux-x86_64
export PATH=$CMAKE_HOME/bin:$PATH
```

3. 激活配置文件并打印CMake版本：

```
source ~/.bashrc
cmake -version
```

### 方法2：源码编译安装

1. 安装编译工具和依赖库

```
sudo apt install g++ make ninja-build unzip libssl-dev
```

2. 下载解压[cmake-3.23.1.tar.gz](https://github.com/Kitware/CMake/releases)并编译安装，默认安装路径在/usr/local/share/cmake-3.23

```
tar -xvf cmake-3.23.1.tar.gz
cd cmake-3.23.1
./configure
make -j32
sudo make install
```

3. 在~/.bash_profile添加环境变量：

```
export PATH=/usr/local/share/cmake-3.23:$PATH
```

4. 运行cmake查看版本

```
cmake -version
```

## MacOS Monterey 12.3.1

### 一、前置要求

1. 安装好xcode开发工具（clang）
2. 确认Command Line Tools for Xcode已经安装

```
xcode-select --version
xcode-select --install
```

3. 安装好brew

```
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install.sh)"
```

### 二、源码编译安装

1. 安装编译工具

```
brew install make clang clang++
```

2. 编译安装

```
tar -xvf cmake-3.23.1.tar.gz
cd cmake-3.23.1
./configure
make -j16
sudo make install
```

3. 查看安装版本

```
cmake --version
```
