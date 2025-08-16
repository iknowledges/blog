# cmake buildsystem

基于CMake的构建系统是指一系列高级逻辑目标(high-level logical targets)。每个目标都对应一个可执行文件、库或者自定义目标。

## 1 二进制目标(Binary Targets)

可执行文件和库分别使用add_executable()和add_library()命令定义。二进制目标之间的依赖关系通过target_link_libraries()命令来表示：

```
# archive被定义为一个静态库
add_library(archive archive.cpp zip.cpp lzma.cpp)
# zipapp被定义为一个可执行文件，并通过编译和链接zipapp.cpp形成
add_executable(zipapp zipapp.cpp)
# 链接zipapp可执行文件时，会链接archive静态库
target_link_libraries(zipapp archive)
```

### 1.1 二进制可执行文件(Binary Executables)

add_executable()命令定义一个可执行目标：

```
add_executable(mytool mytool.cpp)
```

### 1.2 二进制库类型(Binary Library Types)

#### 1.2.1 正常库(Normal Libraries)

add_library()命令默认定义一个静态库，也可以指定类型：

```
add_library(archive SHARED archive.cpp zip.cpp lzma.cpp)
add_library(archive STATIC archive.cpp zip.cpp lzma.cpp)
```

MODULE库是一种运行时作为插件被加载的类型：

```
add_library(archive MODULE 7z.cpp)
```

#### 1.2.2 对象库(Object Libraries)

OBJECT库定义了对象文件的非链接集合(non-archival collection of object files)。通过使用`$<TARGET_OBJECTS:name>`语法，对象文件集合可用作其他目标的输入。

```
add_library(archive OBJECT archive.cpp zip.cpp lzma.cpp)

# archiveExtras的链接步骤除了使用其自身源文件外，还将使用对象文件集
add_library(archiveExtras STATIC $<TARGET_OBJECTS:archive> extras.cpp)

add_executable(test_exe $<TARGET_OBJECTS:archive> test.cpp)
```

对象库也可以链接到其他目标：

```
add_library(archive OBJECT archive.cpp zip.cpp lzma.cpp)

# archiveExtras的链接步骤将直接链接到对象库中的对象文件。
add_library(archiveExtras STATIC extras.cpp)
target_link_libraries(archiveExtras PUBLIC archive)

add_executable(test_exe test.cpp)
target_link_libraries(test_exe archive)
```

## 2 构建规范和使用要求(Build Specification and Usage Requirements)

target_include_directories()、target_compile_definitions()和target_compile_options()命令指定二进制目标的编译规范和使用要求。这些命令分别填充INCLUDE_DIRECTORIES、COMPILE_DEFINITIONS和COMPILE_OPTIONS目标属性，或者INTERFACE_INCLUDE_DIRECTORIES、INTERFACE_COMPILE_DEFINITIONS和INTERFACE_COMPILE_OPTIONS目标属性。

每个命令都有PRIVATE、PUBLIC和INTERFACE三种模式。PRIVATE模式只填充非INTERFACE_变量，INTERFACE模式只填充INTERFACE_变量，PUBLIC模式则都会填充。每条命令可以通过多次使用关键字来调用指定模式：

```
target_compile_definitions(archive
  PRIVATE BUILDING_WITH_LZMA
  INTERFACE USING_ARCHIVE_LIB
)
```

## 2.1 目标属性(Target Properties)

目标属性INCLUDE_DIRECTORIES、COMPILE_DEFINITIONS和COMPILE_OPTIONS的内容在编译二进制源文件时被使用。

INCLUDE_DIRECTORIES中的条目会以-I或-isystem为前缀添加到编译行中。

COMPILE_DEFINITIONS中的条目会以-D或/D为前缀添加到编译行中。

## 2.2 使用要求的传递性(Transitive Usage Requirements)

目标的使用要求可以传递给依赖。target_link_libraries()命令有PRIVATE、INTERFACE和PUBLIC关键字来控制传播。

```
add_library(archive archive.cpp)
target_compile_definitions(archive INTERFACE USING_ARCHIVE_LIB)

add_library(serialization serialization.cpp)
target_compile_definitions(serialization INTERFACE USING_SERIALIZATION_LIB)

add_library(archiveExtras extras.cpp)
# 因为archive是archiveExtras的PUBLIC依赖项，它的使用要求也会传递给cosumer
target_link_libraries(archiveExtras PUBLIC archive)
# 因为serialization是archiveExtras的PRIVATE依赖项，它的使用要求不会传递给cosumer
target_link_libraries(archiveExtras PRIVATE serialization)
# archiveExtras is compiled with -DUSING_ARCHIVE_LIB
# and -DUSING_SERIALIZATION_LIB

add_executable(consumer consumer.cpp)
# consumer is compiled with -DUSING_ARCHIVE_LIB
target_link_libraries(consumer archiveExtras)
```

如果一个依赖不使用库的实现，只使用其头文件，则应将其指定为INTERFACE依赖。

# 4 伪目标(Pseudo Targets)

伪目标类型并不代表构建系统的输出，而只代表输入，如外部依赖关系、别名或其他非构建工件。

## 4.1 Imported Targets

IMPORTED目标表示预先存在的依赖关系。这类目标通常由上游软件包定义，并且不可更改。

## 4.2 Alias Targets

ALIAS目标是一个别名，可以代替二进制目标的名称。

## 4.3 Interface Libraries

INTERFACE库目标不会编译源代码，也不会在磁盘上生成库文件，因此没有LOCATION。

#### 参考资源

- [cmake-buildsystem](https://cmake.org/cmake/help/latest/manual/cmake-buildsystem.7.html)