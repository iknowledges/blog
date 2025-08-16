# cmake packages

## Package Layout

### Package Configuration File

1. 项目Foo安装在如下路径：

```
<prefix>/include/foo-1.2/foo.h
<prefix>/lib/foo-1.2/libfoo.a
```

2. CMake包配置文件路径如下：

```
<prefix>/lib/cmake/foo-1.2/FooConfig.cmake
```

文件内容如下：

```
# ...
# (compute PREFIX relative to file location)
# ...
set(Foo_INCLUDE_DIRS ${PREFIX}/include/foo-1.2)
set(Foo_LIBRARIES ${PREFIX}/lib/foo-1.2/libfoo.a)
```

3. find_package()命令用于搜索包配置文件。给定名称Foo，就会去查找名为FooConfig.cmake或foo-config.cmake的配置文件。该命令会查找的一个路径如下：

```
<prefix>/lib/cmake/Foo*/
```

这样会匹配到路径`<prefix>/lib/cmake/foo-1.2`，从而包配置文件会被找到。


#### 参考资源

- [cmake-packages](https://cmake.org/cmake/help/latest/manual/cmake-packages.7.html)
