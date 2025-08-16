# cmake developer

## 查找模块(Find Modules)

"find module"是指`Find<PackageName>.cmake`文件被find_package()命令加载，从而调用`<PackageName>`包。

## 标准变量名(Standard Variable Names)

- Xxx_INCLUDE_DIRS: 列出包含目录的集合。不能是缓存变量（注意不应作为find_path()命令的结果变量）。
- Xxx_LIBRARIES: 模块需要的库。它们可以是CMake目标库、二进制库文件的绝对路径或链接器搜索路径中的库名。不能是缓存变量（注意不应作为find_library()命令的结果变量）。
- Xxx_INCLUDE_DIR: 当模块只提供一个库时，这个变量可以用来指定在哪里查找该库的头文件。可以作为find_path()命令的结果变量。
- Xxx_LIBRARY: 当模块只提供一个库时，这个变量可以用来指定库的路径。可以作为find_library()命令的结果变量。

## 查找模块的例子(A Sample Find Module)

1. 首先我们尝试使用pkg-config查找库。

```
find_package(PkgConfig)
pkg_check_modules(PC_Foo QUIET Foo)
```

2. 通过pkg-config查找库将会定义一些以PC_Foo_开头的变量，其中包含来自Foo.pc文件的信息。我们使用这些信息为CMake提供提示，告诉它去哪里找库和头文件：

```
find_path(Foo_INCLUDE_DIR
  NAMES foo.h
  PATHS ${PC_Foo_INCLUDE_DIRS}
  PATH_SUFFIXES Foo
)
find_library(Foo_LIBRARY
  NAMES foo
  PATHS ${PC_Foo_LIBRARY_DIRS}
)
```

3. 使用pkg-config提供的信息设置版本号：

```
set(Foo_VERSION ${PC_Foo_VERSION})
```

4. 检查REQUIRED_VARS包含的变量并合理设置Foo_FOUND：

```
include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(Foo
  FOUND_VAR Foo_FOUND
  REQUIRED_VARS
    Foo_LIBRARY
    Foo_INCLUDE_DIR
  VERSION_VAR Foo_VERSION
)
```

5. 有两种方法提供给用户查找模块。传统变量方法(traditional variable approach)如下：

```
if(Foo_FOUND)
  set(Foo_LIBRARIES ${Foo_LIBRARY})
  set(Foo_INCLUDE_DIRS ${Foo_INCLUDE_DIR})
  set(Foo_DEFINITIONS ${PC_Foo_CFLAGS_OTHER})
endif()
```

提供imported target方法如下：

```
if(Foo_FOUND AND NOT TARGET Foo::Foo)
  add_library(Foo::Foo UNKNOWN IMPORTED)
  set_target_properties(Foo::Foo PROPERTIES
    IMPORTED_LOCATION "${Foo_LIBRARY}"
    INTERFACE_COMPILE_OPTIONS "${PC_Foo_CFLAGS_OTHER}"
    INTERFACE_INCLUDE_DIRECTORIES "${Foo_INCLUDE_DIR}"
  )
endif()
```

imported target应使用命名空间（如Foo::前缀），CMake会识别传递给target_link_libraries()的名称中包含::的值。

6. 大部分缓存变量在ccmake界面上会被隐藏，除非用户明确要求编辑它们：

```
mark_as_advanced(
  Foo_INCLUDE_DIR
  Foo_LIBRARY
)
```

#### 参考资源

- [cmake-developer](https://cmake.org/cmake/help/latest/manual/cmake-developer.7.html)