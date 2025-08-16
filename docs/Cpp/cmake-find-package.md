# CMake的find_package函数

Linux中使用如下代码引入OpenCV依赖：

```
add_executable(my_demo src/my_demo.cpp)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
target_link_libraries(my_demo, ${OpenCV_LIBS})
```

工作流程如下：

1. find_package在一些目录中查找OpenCV的配置文件；
2. 找到后，find_package会将头文件目录设置到`${OpenCV_INCLUDE_DIRS}`中，将链接库设置到`${OpenCV_LIBS}`中；
3. 设置可执行文件的头文件目录和链接库，然后开始编译。

## find_package

find_package包括两种搜索包的模式：

- 模块模式(Module mode)：这个模式下CMake查找名为`Find<PackageName>.cmake`的文件，首先在`CMAKE_MODULE_PATH`中列出的位置查找，然后在CMake安装时提供的[Find Modules](https://cmake.org/cmake/help/latest/manual/cmake-developer.7.html#find-modules)中查找。
- 配置模式(Config mode)：这个模式下CMake会搜索名为`<lowercasePackageName>-config.cmake`或`<PackageName>Config.cmake`的文件。如果指定了版本号还会查找`<lowercasePackageName>-config-version.cmake`或`<PackageName>ConfigVersion.cmake`文件。

basic signature和full signature搜索包的方式不同:

- basic signature: 使用基本签名时，命令首先使用模块模式搜索。如果没找到软件包，则使用配置模式搜索。用户可将变量CMAKE_FIND_PACKAGE_PREFER_CONFIG设置为true，以反转优先级。也可以使用MODULE关键字强制基本签名只使用模块模式。
- full signature: 如果使用完整签名，命令只能在配置模式下搜索。

### Basic Signature

```
find_package(<PackageName> [version] [EXACT] [QUIET] [MODULE]
             [REQUIRED] [[COMPONENTS] [components...]]
             [OPTIONAL_COMPONENTS components...]
             [REGISTRY_VIEW  (64|32|64_32|32_64|HOST|TARGET|BOTH)]
             [GLOBAL]
             [NO_POLICY_SCOPE]
             [BYPASS_PROVIDER])
```

无论使用哪种模式，变量`<PackageName>_FOUND`都会被设置，以指示是否找到了软件包。

- QUIET：禁用信息类消息，包括无法找到软件包的消息。
- REQUIRED: 如果找不到软件包，会以错误信息停止处理。
- COMPONENTS: 列出软件包所需的组件列表。如果存在REQUIRED选项，则可以省略COMPONENTS关键字，直接在REQUIRED后列出组件。
- OPTIONAL_COMPONENTS: 列出可选的组件。
- REGISTRY_VIEW: 指定应查询哪些注册表视图(registry views)。该关键字只在Windows平台上有效。(version 3.24)
- GLOBAL: 导入项目时将所有被导入的目标提升到全局范围。也可以通过设置CMAKE_FIND_PACKAGE_TARGETS_GLOBAL变量来启用此功能。(version 3.24)
- version: 指定软件包的版本号。
- EXACT: 要求精确匹配版本。

### Full Signature

```
find_package(<PackageName> [version] [EXACT] [QUIET]
             [REQUIRED] [[COMPONENTS] [components...]]
             [OPTIONAL_COMPONENTS components...]
             [CONFIG|NO_MODULE]
             [GLOBAL]
             [NO_POLICY_SCOPE]
             [BYPASS_PROVIDER]
             [NAMES name1 [name2 ...]]
             [CONFIGS config1 [config2 ...]]
             [HINTS path1 [path2 ... ]]
             [PATHS path1 [path2 ... ]]
             [REGISTRY_VIEW  (64|32|64_32|32_64|HOST|TARGET|BOTH)]
             [PATH_SUFFIXES suffix1 [suffix2 ...]]
             [NO_DEFAULT_PATH]
             [NO_PACKAGE_ROOT_PATH]
             [NO_CMAKE_PATH]
             [NO_CMAKE_ENVIRONMENT_PATH]
             [NO_SYSTEM_ENVIRONMENT_PATH]
             [NO_CMAKE_PACKAGE_REGISTRY]
             [NO_CMAKE_BUILDS_PATH] # Deprecated; does nothing.
             [NO_CMAKE_SYSTEM_PATH]
             [NO_CMAKE_INSTALL_PREFIX]
             [NO_CMAKE_SYSTEM_PACKAGE_REGISTRY]
             [CMAKE_FIND_ROOT_PATH_BOTH |
              ONLY_CMAKE_FIND_ROOT_PATH |
              NO_CMAKE_FIND_ROOT_PATH])
```

配置模式尝试查找软件包提供的配置文件。系统会创建一个名为`<PackageName>_DIR`的缓存变量来保存包含该文件的目录。

配置文件的完整路径保存在cmake变量`<PackageName>_CONFIG`中。

- CONFIG或NO_MODULE: 强制执行纯配置模式。
- NAMES: 默认情况下，命令会搜索名为`<PackageName>`的软件包。如果给定了NAMES选项，则会使用该名称来代替`<PackageName>`。
- CONFIGS: 命令搜索配置文件的默认名称为`<PackageName>Config.cmake`或`<lowercasePackageName>-config.cmake`。可以使用CONFIGS选项来替换配置文件名。

### 配置模式搜索过程

下标显示了CMake可能会搜索的目录，每个条目适用于Windows(W)、UNIX(U)或Apple(A)：

| Entry | Convention |
| ----- | ----- |
| `<prefix>/` | W |
| `<prefix>/(cmake\|CMake)/` | W |
| `<prefix>/<name>*/` | W |
| `<prefix>/<name>*/(cmake\|CMake)/` | W |
| `<prefix>/<name>*/(cmake\|CMake)/<name>*/` | W |
| `<prefix>/(lib/<arch>\|lib*\|share)/cmake/<name>*/` | U |
| `<prefix>/(lib/<arch>\|lib*\|share)/<name>*/` | U |
| `<prefix>/(lib/<arch>\|lib*\|share)/<name>*/(cmake\|CMake)/` | U |
| `<prefix>/<name>*/(lib/<arch>\|lib*\|share)/cmake/<name>*/` | W/U |
| `<prefix>/<name>*/(lib/<arch>\|lib*\|share)/<name>*/` | W/U |
| `<prefix>/<name>*/(lib/<arch>\|lib*\|share)/<name>*/(cmake\|CMake)/` | W/U |

所有条目中的`<name>`都不区分大小写，并与`<PackageName>`或`NAMES`给出的名称相对应。

指定架构的`lib/<arch>`和`lib*`目录将按照以下规则搜索：

- `lib/<arch>`: 如果设置了`CMAKE_LIBRARY_ARCHITECTURE`变量才会搜索。
- `lib64`: 在64位平台上（`CMAKE_SIZEOF_VOID_P`为8）且`FIND_LIBRARY_USE_LIB64_PATHS`属性设置为true才搜索。
- `lib32`: 在32位平台上（`CMAKE_SIZEOF_VOID_P`为4）且`FIND_LIBRARY_USE_LIB32_PATHS`属性设置为true才搜索。
- `libx32`: 如果`FIND_LIBRARY_USE_LIBX32_PATHS`属性设置为true，则在x32 ABI平台上进行搜索。
- `lib`: 总会搜索。

前缀`<prefix>`的创建创建步骤如下：

1. 搜索当前`<PackageName>`所独有的前缀，由变量`<PackageName>_ROOT`指定。参见策略CMP0074。（version 3.12）
2. 由如下cmake的缓存变量指定搜索路径。这些变量应在命令行中以`-DVAR=VALUE`的形式使用：
   - `CMAKE_PREFIX_PATH`
   - `CMAKE_FRAMEWORK_PATH`
   - `CMAKE_APPBUNDLE_PATH`
3. 由如下cmake的环境变量指定搜索路径：
   - `<PackageName>_DIR`
   - `CMAKE_PREFIX_PATH`
   - `CMAKE_FRAMEWORK_PATH`
   - `CMAKE_APPBUNDLE_PATH`
4. 由`HINTS`选项指定搜索路径。
5. 搜索标准的系统环境变量：`PATH`。
6. 搜索存储在[User Package Registry](https://cmake.org/cmake/help/latest/manual/cmake-packages.7.html#user-package-registry)中的路径。
7. 搜索当前系统Platform文件中定义的cmake变量：
   - CMAKE_SYSTEM_PREFIX_PATH
   - CMAKE_SYSTEM_FRAMEWORK_PATH
   - CMAKE_SYSTEM_APPBUNDLE_PATH
   这些变量指定的平台路径通常包括已安装软件的位置。例如UNIX平台的/usr/local。
8. 搜索存储在[System Package Registry](https://cmake.org/cmake/help/latest/manual/cmake-packages.7.html#system-package-registry)中的路径。
9. 由`PATHS`选项指定搜索路径。

## 可执行文件如何寻找动态库

#### 参考资源

- [cmake find_package路径详解](https://zhuanlan.zhihu.com/p/50829542)
- [find_package](https://cmake.org/cmake/help/latest/command/find_package.html)