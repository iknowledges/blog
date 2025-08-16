# Ubuntu编译calibre

calibre项目源码地址：https://github.com/kovidgoyal/calibre

## 准备环境

安装Qt并配置环境变量：

```
# CMake
export PATH=/home/sheeran/Desktop/software/Qt/Tools/CMake/bin:$PATH

# QMake
export PATH=/home/sheeran/Desktop/software/Qt/6.5.2/gcc_64/bin:$PATH
```

## 安装依赖

https://github.com/kovidgoyal/calibre/blob/master/bypy/sources.json

## 编译命令

```
python setup.py bootstrap
```