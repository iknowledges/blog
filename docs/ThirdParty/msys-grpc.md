# MSYS2编译Grpc项目

1. 项目CMake配置如下：

```
cmake_minimum_required(VERSION 3.30)
project(GrpcDemo)

set(CMAKE_CXX_STANDARD 17)

find_package(absl CONFIG REQUIRED)
message(STATUS "Using absl ${absl_VERSION}")

# Protobuf
find_package(Protobuf CONFIG REQUIRED)
message(STATUS "Using protobuf ${Protobuf_VERSION}")
# gRPC
find_package(gRPC CONFIG REQUIRED)
message(STATUS "Using gRPC ${gRPC_VERSION}")

add_executable(GrpcDemo main.cc
        helloworld.grpc.pb.cc
        helloworld.grpc.pb.h
        helloworld.pb.cc
        helloworld.pb.h)

target_link_libraries(${PROJECT_NAME} PRIVATE protobuf::libprotobuf gRPC::grpc++_reflection gRPC::grpc++ absl::base absl::strings)
```

2. 安装grpc

```
pacman -S mingw-w64-ucrt-x86_64-grpc
```

3. 生成protobuf文件

```
# generate service codes
protoc -I ./protos --grpc_out=. --plugin=protoc-gen-grpc=/ucrt64/bin/grpc_cpp_plugin.exe ./protos/strategy.proto
# generate message codes
protoc -I ./protos --cpp_out=. ./protos/strategy.proto
```

4. 开始编译

```
cmake -B build .
cmake --build build/
```

5. 最后使用Enigma Virtual Box工具打包，可将exe程序和/ucrt64/bin下的动态库打包到一起。

#### 参考资料

- [Enigma Virtual Box用法exe封包工具介绍](https://blog.csdn.net/ffffffeiyu/article/details/137087625)
