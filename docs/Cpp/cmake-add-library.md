# CMake的add_library函数

## Normal Libraries

```
add_library(<name> [<type>] [EXCLUDE_FROM_ALL] <sources>...)
```

添加名为<name>的目标库，并根据命令调用sources中列出的源文件进行构建。

<type>指定要创建的库类型：

- STATIC: 静态库
- SHARED: 动态库
- MODULE: 模块库

EXCLUDE_FROM_ALL：可使这个库排除在all之外，即必须明确点击生成才会生成。

<name>在一个项目中必须是全局唯一的。构建的库的实际文件名是根据本地平台进行构建（如 lib<name>.a 或 <name>.lib）。

3.11版新增功能：如果以后使用target_sources()添加源文件，则可省略源文件。

默认情况下，库文件将输出到以下几个目录ARCHIVE_OUTPUT_DIRECTORY、LIBRARY_OUTPUT_DIRECTORY和RUNTIME_OUTPUT_DIRECTORY，最终生成的文件名由OUTPUT_NAME属性确定。


## Object Libraries

```
add_library(<name> OBJECT <sources>...)
```

添加对象库，编译源文件时不将对象文件链接到库中。

通过add_library或add_executable()创建的其他目标可以使用 $<TARGET_OBJECTS:objlib> 的形式引用对象，其中 objlib 是对象库名称。例如：

```
add_library(... $<TARGET_OBJECTS:objlib> ...)
add_executable(... $<TARGET_OBJECTS:objlib> ...)
```

## Interface Libraries

```
add_library(<name> INTERFACE)
```

添加接口库目标，该目标可指定依赖项的使用要求，但不编译源代码，也不在磁盘上生成库工件。

在构建系统中，没有源文件的接口库不会被列为目标。不过，它可以被设置属性，也可以被安装和导出。通常，INTERFACE_* 属性是使用以下命令在接口目标上填充的：

- set_property()
- target_link_libraries(INTERFACE)
- target_link_options(INTERFACE)
- target_include_directories(INTERFACE)
- target_compile_options(INTERFACE)
- target_compile_definitions(INTERFACE)
- target_sources(INTERFACE)

然后它就可以像其他目标一样被用作 target_link_libraries() 的参数。

3.19版新增功能：

```
add_library(<name> INTERFACE [EXCLUDE_FROM_ALL] <sources>...)
```

添加带有源文件的接口库目标。源文件可以直接在 add_library 中列出，也可以通过调用带有 PRIVATE 或 PUBLIC 关键字的 target_sources() 来添加。

如果接口库有源文件（即设置了 SOURCES 目标属性）或头文件集（即设置了 HEADER_SETS 目标属性），它将作为编译目标出现在编译系统中，就像 add_custom_target() 命令定义的目标一样。它不会编译任何源代码，但包含 add_custom_command() 自定义命令创建的编译规则。

## Imported Libraries

```
add_library(<name> <type> IMPORTED [GLOBAL])
```

该目标库可以像项目中构建的任何目标库一样被引用，但默认情况下只能在创建该目标库的目录及其下级目录中可见。

<type> 必须是以下选项之一：

- STATIC, SHARED, MODULE, UNKNOWN
- OBJECT
- INTERFACE

GLOBAL：使目标名称全局可见。

在构建导入目标时不会生成任何规则，且 IMPORTED 目标属性为 True。导入的库可以方便地从 target_link_libraries() 等命令中引用。

## Alias Libraries

```
add_library(<name> ALIAS <target>)
```

创建别名目标，这样 <name> 就可以在后续命令中引用 <target>。<name> 不会作为 make 目标出现在生成的构建系统中。<target> 不能是 ALIAS。

#### 参考资料

- [add_library](https://cmake.org/cmake/help/latest/command/add_library.html)