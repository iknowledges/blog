# Pyinstaller使用教程

### 安装python

在Ubuntu下使用Pyinstaller，在编译Python时要加上`--enable-shared`，因为Pyinstaller打包时要使用python的动态库。打包时如果要使用`--splash`选项，那么在编译时就要安装tkinter。

```
./configure --enable-shared
make -j 4
sudo make install
```

### 安装Pyinstaller

```
pip3 install pyinstaller
```

打包命令：

```
pyinstaller -F -w main.py --splash splash.png
```

### 错误处理

- 执行Pyinstaller打包时报错如下：

```
error while loading shared libraries: libpython3.11.so.1.0
```

解决方法：将缺少的动态库拷贝到/usr/lib目录下

```
find / -name libpython3.11.so.1.0
sudo cp /usr/local/lib/libpython3.11.so.1.0 /usr/lib
```

#### 参考资料

- [pyinstaller说明(windows、mac、linux)](https://blog.csdn.net/qq_41004932/article/details/118995838)
