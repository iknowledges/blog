# 编译Okular

各种系统支持的Okular版本：https://repology.org/project/okular/versions

## MSYS2

1. 安装依赖

```
pacman -S mingw-w64-ucrt-x86_64-extra-cmake-modules
pacman -S mingw-w64-ucrt-x86_64-kf5
# RQUIRED packages
pacman -S mingw-w64-ucrt-x86_64-libspectre
pacman -S mingw-w64-ucrt-x86_64-libkexiv2
pacman -S mingw-w64-ucrt-x86_64-chmlib
pacman -S mingw-w64-ucrt-x86_64-djvulibre
pacman -S mingw-w64-ucrt-x86_64-kdegraphics-mobipocket
pacman -S mingw-w64-ucrt-x86_64-discount
```

2. 下载源码进行编译

```
git clone https://invent.kde.org/graphics/okular.git
cd okular
git checkout v23.08.5
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/path/to/okular/install -G "MinGW Makefiles" ..
mingw32-make -j8
mingw32-make install
```

3. 启动程序

```
source build/prefix.sh
install/bin/okular.exe
```

#### 问题解决

- 启动程序报错：无法找到Okular组件：共享库没有被找到。

使用命令查看`echo $QT_PLUGIN_PATH`，发现末尾多了一个冒号，修改prefix.sh，将冒号后面的内容删除，然后重新设置环境变量再启动程序。

## Ubuntu

1. 安装依赖，参考：https://packages.ubuntu.com/source/jammy/okular

```
sudo apt install extra-cmake-modules
sudo apt install qtbase5-dev libqt5svg5-dev qtdeclarative5-dev libphonon4qt5-dev libphonon4qt5experimental-dev
sudo apt install libkf5activities-dev libkf5archive-dev libkf5bookmarks-dev libkf5bookmarks-dev libkf5completion-dev libkf5config-dev libkf5configwidgets-dev libkf5coreaddons-dev libkf5crash-dev libkf5doctools-dev libkf5i18n-dev libkf5iconthemes-dev libkf5kexiv2-dev libkf5khtml-dev libkf5kio-dev libkf5kjs-dev libkf5parts-dev libkf5pty-dev libkf5purpose-dev libkf5textwidgets-dev libkf5threadweaver-dev libkf5wallet-dev libkf5windowsystem-dev
sudo apt install zlib1g-dev
sudo apt install libpoppler-qt5-dev
sudo apt install libepub-dev
sudo apt install breeze-icon-theme
```

2. 下载源码进行编译

```
git clone https://invent.kde.org/graphics/okular.git
cd okular
git checkout v21.12.3
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=~/Desktop/okular ..
make -j8
make install
```

3. 启动程序

```
source build/prefix.sh
bin/okular
```

### 使用VSCode进行编译

1. 添加CMakePresets.json文件：

```json
{
    "version": 2,
    "configurePresets": [
        {
            "name": "default",
            "displayName": "Custom configure preset",
            "description": "Sets Unix Makefiles generator",
            "generator": "Unix Makefiles",
            "binaryDir": "${sourceDir}/out/build/${presetName}",
            "cacheVariables": {
                "CMAKE_BUILD_TYPE": "Debug",
                "CMAKE_INSTALL_PREFIX": "${sourceDir}/out/install/${presetName}",
                "CMAKE_C_COMPILER": "gcc",
                "CMAKE_CXX_COMPILER": "g++",
                "CMAKE_MAKE_PROGRAM": "make"
            }
        }
    ]
}
```

2. 执行完CMake: Configure和CMake: Build命令后，进入out/build/default目录，执行如下命令：

```
make install
```

3. 在~/.bashrc下添加如下内容：

```
source /path/to/okular/out/build/default/prefix.sh
```

4. 不使用CMakePresets.json的编译命令：

```
# Configure
/usr/bin/cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=/home/abc/workspace/okular/out/install/default -DCMAKE_C_COMPILER=gcc -DCMAKE_CXX_COMPILER=g++ -DCMAKE_MAKE_PROGRAM=make -S/home/abc/workspace/okular -B/home/abc/workspace/okular/out/build/default -G "Unix Makefiles"
# Build
/usr/bin/cmake --build /home/abc/workspace/okular/out/build/default --parallel 14
# Install
/usr/bin/cmake --install /home/abc/workspace/okular/out/build/default
```

#### 参考资料

- [在Linux上从源代码编译Okular](https://okular.kde.org/zh-cn/build-it/)
