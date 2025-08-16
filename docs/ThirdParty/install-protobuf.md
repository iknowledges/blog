# Protocol Buffers源码安装教程

### Windows安装

1. 下载protobuf及其依赖的子模块，30以上版本的`.gitmodules`文件是空的，无法下载子模块。

```
git clone -b v29.4 git@github.com:protocolbuffers/protobuf.git
cd protobuf
git submodule update --init --recursive
```

2. 打开cmake-gui进行如下设置，再依次点击Configure和Generate：

- Where is the source code: F:/third_party_build/protobuf
- Where to build the binaries: F:/third_party_build/protobuf/build
- CMAKE_INSTALL_PREFIX: F:/third_party/protobuf
- 建议取消勾选BUILD_TESTING和protobuf_BUILD_TESTS

3. 用Visual Studio打开build目录下生成的protobuf.sln工程，选中所有要编译的项目右键【属性】，找到【C/C++】->【代码生成】->【运行库】，将/MTd改成/MDd，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

4. 最后编写CMakeLists.txt

```
list(APPEND CMAKE_PREFIX_PATH $ENV{THIRD_PARTY_DIR})

# Protobuf
find_package(Protobuf CONFIG REQUIRED)
message(STATUS "Using protobuf ${Protobuf_VERSION}")

target_link_libraries(${PROJECT_NAME} PRIVATE protobuf::libprotobuf)
```

### 代码示例

1. 编写person.proto文件：

```
syntax = "proto3";

package tutorial;

message Person {
    string name = 1;
    int32 id = 2;
}
```

2. 生成代码：

```
F:\third_party\protobuf\bin\protoc.exe -I./proto --cpp_out=./generated person.proto
```

3. 编写测试代码：

```cpp
#include <iostream>
#include "generated/person.pb.h"

int main() {
    std::string s;
    tutorial::Person p1;
    p1.set_id(1);
    p1.set_name("Apple");
    if (!p1.SerializePartialToString(&s)) {
        std::cerr << "Serialize Failed!" << std::endl;
        return -1;
    }
    std::cout << "Serialize person: " << s << std::endl;

    tutorial::Person p2;
    if (!p2.ParseFromString(s)) {
        std::cerr << "Deserialize Failed!" << std::endl;
        return -1;
    }
    std::cout << "Deserialize person name: " << p2.name() << std::endl;
    return 0;
}
```

#### 参考资料

- [Replaced CMake Submodules with Fetched Deps](https://protobuf.dev/support/migration/)
- [在Windows上使用CMake编译Protobuf](https://zhuanlan.zhihu.com/p/721433031)
