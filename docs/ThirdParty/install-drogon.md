# Drogon源码编译安装教程

## Drogon安装

1. 下载源代码

```
git clone https://github.com/drogonframework/drogon
cd drogon
git submodule update --init
```

2. 安装依赖库

```
mkdir build
cd build/
conan install .. --output-folder=. --build=missing --profile=debug
```

- 为了支持MySQL，需要在源代码的conanfile.txt中添加`mariadb-connector-c/3.4.3`
- 为了解决如下报错，需要在源代码的conanfile.txt中添加`libuuid/1.0.3`：

```
CMake Error at cmake_modules/FindUUID.cmake:98 (message):
  Could not find UUID
Call Stack (most recent call first):
  CMakeLists.txt:222 (find_package)
```

- conanfile.txt：

```
[requires]
jsoncpp/1.9.4
zlib/1.2.11
gtest/1.10.0
sqlite3/3.40.1
#libpq/13.2
openssl/1.1.1t
hiredis/1.0.0
brotli/1.0.9
libuuid/1.0.3
mariadb-connector-c/3.4.3

[generators]
CMakeToolchain

[options]

[imports]
```

3. 编译安装

```
cmake .. -DCMAKE_TOOLCHAIN_FILE=conan_toolchain.cmake  -DCMAKE_POLICY_DEFAULT_CMP0091=NEW -DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=/path/to/third_party/drogon

cmake --build . --parallel --target install
```

## CMake项目中使用Drogon

1. 在项目根目录创建conanfile.txt，内容如下：

```
[requires]
jsoncpp/1.9.4
zlib/1.2.11
gtest/1.10.0
sqlite3/3.40.1
openssl/1.1.1t
hiredis/1.0.0
brotli/1.0.9
libuuid/1.0.3
mariadb-connector-c/3.4.3

[generators]
CMakeToolchain
```

2. 安装依赖

```
conan install . -of=build/ -b=missing -pr=debug
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
