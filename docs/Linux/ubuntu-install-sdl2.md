# Ubuntu20.04安装SDL2

### 编译安装步骤

```bash
git clone https://github.com/libsdl-org/SDL.git -b SDL2
cd SDL
mkdir build
cd build
../configure
make
sudo make install
```

### 配置环境变量

打开`~/.bashrc`加入下面内容：

```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/sheeran/Desktop/Software/SDL/build/build/.libs
```

然后执行命令：

```
source ~/.bashrc
```

#### 参考资料

[Installing SDL](https://wiki.libsdl.org/SDL2/Installation)