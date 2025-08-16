# Ubuntu排查PyQt运行错误

1. 在代码中启用调试信息

```
os.environ["QT_DEBUG_PLUGINS"] = "1"
```

2. 再次运行出现如下错误：

```
Cannot load library /home/sheeran/Desktop/Software/venv/lector/lib/python3.10/site-packages/PyQt5/Qt5/plugins/platforms/libqxcb.so: (libxcb-xinerama.so.0: 无法打开共享对象文件: 没有那个文件或目录)
QLibraryPrivate::loadPlugin failed on "/home/sheeran/Desktop/Software/venv/lector/lib/python3.10/site-packages/PyQt5/Qt5/plugins/platforms/libqxcb.so" : "Cannot load library /home/sheeran/Desktop/Software/venv/lector/lib/python3.10/site-packages/PyQt5/Qt5/plugins/platforms/libqxcb.so: (libxcb-xinerama.so.0: 无法打开共享对象文件: 没有那个文件或目录)"
```

3. 安装缺失的依赖

```
sudo apt install libxcb-xinerama0
```