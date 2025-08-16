# gRPC源码编译安装教程

## Ubuntu

1. 设置环境变量

```
export MY_INSTALL_DIR=$HOME/.local
export PATH="$MY_INSTALL_DIR/grpc/bin:$PATH"
```

2. 安装编译工具及第三方依赖

```
sudo apt install -y cmake
sudo apt install -y build-essential autoconf libtool pkg-config
```

3. 克隆仓库并开始编译

```
git clone --recurse-submodules -b v1.64.0 --depth 1 --shallow-submodules https://github.com/grpc/grpc
cd grpc
mkdir -p cmake/build
cd cmake/build
cmake -DgRPC_INSTALL=ON \
      -DgRPC_BUILD_TESTS=OFF \
      -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR/grpc \
      ../..
make -j 8
make install
```

## Windows

1. 下载gRPC源码

```
git config --global url."git@github.com:".insteadOf "https://github.com/"
git clone --recurse-submodules -b v1.66.0 --depth 1 --shallow-submodules https://github.com/grpc/grpc
git config --global --unset url."git@github.com:".insteadOf
```

2. 打开cmkae-gui，并进行如下配置：

- Where is the source code: `F:/third_party_build/grpc`
- Where to build the binaries: `F:/third_party_build/grpc/cmake_build`

点击Configure，然后修改变量：

- CMAKE_INSTALL_PREFIX: `F:/third_party/grpc`
- INSTALL_BIN_DIR: `F:/third_party/grpc/bin`
- INSTALL_INC_DIR: `F:/third_party/grpc/include`
- INSTALL_LIB_DIR: `F:/third_party/grpc/lib`
- INSTALL_MAN_DIR: `F:/third_party/grpc/share/man`
- INSTALL_PKGCONFIG_DIR: `F:/third_party/grpc/share/pkgconfig`

然后点击Generate。

3. 用Visual Studio打开build目录下生成的grpc.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

## CMake配置使用gRPC

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR}/grpc)

# Protobuf
find_package(Protobuf CONFIG REQUIRED)
message(STATUS "Using protobuf ${Protobuf_VERSION}")
# gRPC
find_package(gRPC CONFIG REQUIRED)
message(STATUS "Using gRPC ${gRPC_VERSION}")

target_link_libraries(${PROJECT_NAME} PRIVATE protobuf::libprotobuf gRPC::grpc++_reflection gRPC::grpc++)
```

#### 参考资料

- [Quick start](https://grpc.io/docs/languages/cpp/quickstart/)
- [gRPC C++](https://github.com/grpc/grpc/tree/master/src/cpp#to-start-using-grpc-c)
