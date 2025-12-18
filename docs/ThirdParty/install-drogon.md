# Drogon源码编译安装教程

## Drogon安装

1. 下载源代码

```
git clone https://github.com/drogonframework/drogon
cd drogon
git submodule update --init
```

2. 安装依赖库，drogon必须的依赖库是jsoncpp、libuuid和zlib，其他依赖都是可选的。我们这里使用MySQL，所以另外添加mariadb-connector-c，并且为了drogon支持HTTPS，添加了openssl。最后修改源代码中的conanfile.txt如下：

```
[requires]
jsoncpp/1.9.4
zlib/1.2.11
libuuid/1.0.3
openssl/1.1.1t
mariadb-connector-c/3.4.3
#sqlite3/3.40.1
#libpq/13.2
#hiredis/1.0.0
#brotli/1.0.9
#gtest/1.10.0

[generators]
CMakeToolchain

[options]

[imports]
```

使用conan安装依赖：

```
mkdir build
cd build/
conan install .. --output-folder=. --build=missing --profile=debug
```

3. 编译安装

```
cmake .. -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=/path/to/third_party/drogon

cmake --build . --parallel --target install
```

4. 将/path/to/third_party/drogon/bin加入PATH后，验证是否安装成功：

```
drogon_ctl version
```

## CMake项目中使用Drogon

1. 在项目根目录创建conanfile.txt，内容和安装时的依赖库一样：

```
[requires]
jsoncpp/1.9.4
zlib/1.2.11
libuuid/1.0.3
openssl/1.1.1t
mariadb-connector-c/3.4.3

[generators]
CMakeToolchain
```

2. 安装依赖库

```
conan install . --output-folder=cmake-build-debug/ --build=missing --profile=debug
```

3. 编写CMakeLists.txt

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR}/drogon)

find_package(Drogon CONFIG REQUIRED)
message(STATUS "Using Drogon ${Drogon_VERSION}")


target_link_libraries(${PROJECT_NAME} PRIVATE Drogon::Drogon)
```

CMake生成项目的命令要加上`-DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake`

#### 参考资料

- [Drogon官方文档](https://drogonframework.github.io/drogon-docs)
