# Ubuntu20.04安装qt6问题解决

## 运行`qt-unified-linux-x64-4.6.0-online.run`安装包报错

```
./qt-unified-linux-x64-4.6.0-online.run: error while loading shared libraries: libxcb-xinerama.so.0: cannot open shared object file: No such file or directory
```

**解决方法：**

```
sudo apt install libxcb-xinerama0
```

## 启动项目报错

新建qmake项目，启动时报下面错误：

```
qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "" even though it was found. 
This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem. 

Available platform plugins are: offscreen, vnc, wayland, vkkhrdisplay, linuxfb, minimal, xcb, wayland-egl, eglfs, minimalegl.
```

解决方法：

1. 在`~/.bashrc`文件中添加如下内容：

```
export QT_DEBUG_PLUGINS=1
```

2. 进入Qt安装目录Qt/Tools/QtCreator/bin/启动QtCreator

```
./qtcreator
```

3. 运行之前创建的项目，控制台会打印如下信息：

```
qt.core.library: "/home/sheeran/Desktop/Software/Qt/6.5.2/gcc_64/plugins/platforms/libqxcb.so" cannot load: Cannot load library /home/sheeran/Desktop/Software/Qt/6.5.2/gcc_64/plugins/platforms/libqxcb.so: (libxcb-cursor.so.0: 无法打开共享对象文件: 没有那个文件或目录)
qt.core.plugin.loader: QLibraryPrivate::loadPlugin failed on "/home/sheeran/Desktop/Software/Qt/6.5.2/gcc_64/plugins/platforms/libqxcb.so" : "Cannot load library /home/sheeran/Desktop/Software/Qt/6.5.2/gcc_64/plugins/platforms/libqxcb.so: (libxcb-cursor.so.0: 无法打开共享对象文件: 没有那个文件或目录)"
```

4. 找到上面的libqxcb.so文件，运行如下命令：

```
ldd libqxcb.so
```

显示没有找到`libxcb-cursor.so.0`

```
linux-vdso.so.1 (0x00007ffccb394000)
libQt6XcbQpa.so.6 => /home/sheeran/Desktop/Software/Qt/6.5.2/gcc_64/plugins/platforms/./../../lib/libQt6XcbQpa.so.6 (0x00007f481d3db000)
libxkbcommon-x11.so.0 => /lib/x86_64-linux-gnu/libxkbcommon-x11.so.0 (0x00007f481d3c1000)
libxkbcommon.so.0 => /lib/x86_64-linux-gnu/libxkbcommon.so.0 (0x00007f481d37f000)
libxcb-cursor.so.0 => not found
libxcb-icccm.so.4 => /lib/x86_64-linux-gnu/libxcb-icccm.so.4 (0x00007f481d378000)
```

5. 安装缺失的依赖，问题解决。

```
sudo apt install libxcb-cursor0
```
