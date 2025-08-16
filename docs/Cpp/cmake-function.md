# cmake常用函数

## Scripting Commands


## Project Commands

### add_executable

- Normal Executables

```
add_executable(<name> [WIN32] [MACOSX_BUNDLE]
               [EXCLUDE_FROM_ALL]
               [source1] [source2 ...])
```

添加名为`<name>`的可执行目标文件，该目标文件将根据命令中列出的源文件(source1、source2)构建。

- Imported Executables

```
add_executable(<name> IMPORTED [GLOBAL])
```

IMPORTED可执行目标(IMPORTED executable target)引用位于项目之外的可执行文件。

- Alias Executables

```
add_executable(<name> ALIAS <target>)
```

创建别名目标(Alias Target)，这样在后续命令中`<name>`就可以指代`<target>`。

### add_library

```
# 添加子目录模块
add_subdirectory(<子模块路径>)
# A项目链接依赖B项目
target_link_libraries(<A项目名> PUBLIC <B项目名>)
# 项目指定头文件位置
target_include_directories(<项目名> PUBLIC <头文件夹相对路径>)
# 添加库的源代码，SHARED动态库，STATIC静态库
add_library(<项目名> SHARED a.cpp)
# 添加测试用例并执行后面的命令
add_test(NAME <测试用例名> COMMAND <命令>)
# 将目录下所有的源文件保存到一个变量中
aux_source_directory(<目录> <变量>)
```

### add_custom_command

```
# PRE_BUILD编译前拷贝 POST_BUILD编译后拷贝
# CMAKE_SOURCE_DIR是CMakeLists.txt所在的源码目录
# CMAKE_BINARY_DIR是编译后的build目录
add_custom_command(
  TARGET ${PROJECT_NAME} POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_SOURCE_DIR}/static ${CMAKE_BINARY_DIR}/static
)
```

#### 参考资料

- [cmake-commands](https://cmake.org/cmake/help/latest/manual/cmake-commands.7.html)