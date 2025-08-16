# ZeroMQ源码编译安装教程

## Windows下安装

### 编译libzmq

1. 下载[zeromq-4.3.5.zip](https://github.com/zeromq/libzmq/releases)源码并解压。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/zeromq-4.3.5
- Where to build the binaries: F:/third_party_build/zeromq-4.3.5/build
- CMAKE_INSTALL_PREFIX: F:/third_party/ZeroMQ

3. 用Visual Studio打开build目录下生成的ZeroMQ.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

#### CMakeList.txt中使用libzmq

```
list(APPEND CMAKE_PREFIX_PATH $ENV{THIRD_PARTY_DIR})

find_package(ZeroMQ CONFIG REQUIRED)
message(STATUS "Using ZeroMQ ${ZeroMQ_VERSION}")

target_link_libraries(${PROJECT_NAME} libzmq)
# Or use static library to link
target_link_libraries(${PROJECT_NAME} libzmq-static)
```

### 编译cppzmq

1. 下载[v4.10.0.zip](https://github.com/zeromq/cppzmq/releases)源码并解压。

2. 打开cmake-gui，点击Add Entry，添加下列变量：

- ZeroMQ_DIR: F:\third_party\ZeroMQ\CMake
- CPPZMQ_BUILD_TESTS: OFF

注：不设置CPPZMQ_BUILD_TESTS=OFF就会去找Catch2测试框架

2. 在cmake-gui进行如下设置，并依次点击Configure和Generate

- Where is the source code: F:/third_party_build/cppzmq-4.10.0
- Where to build the binaries: F:/third_party_build/cppzmq-4.10.0/build
- CMAKE_INSTALL_PREFIX: F:/third_party/cppzmq

3. 用Visual Studio打开build目录下生成的cppzmq.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

#### CMakeList.txt中使用cppzmq

```
list(APPEND CMAKE_PREFIX_PATH $ENV{THIRD_PARTY_DIR})

find_package(cppzmq CONFIG REQUIRED)
message(STATUS "Using cppzmq ${cppzmq_VERSION}")

target_link_libraries(${PROJECT_NAME} cppzmq)
# Or use static library to link
target_link_libraries(${PROJECT_NAME} cppzmq-static)
```

## 多线程中使用libzmq

- server.cpp

```cpp
#include <zmq.h>
#include <vector>
#include <thread>
#include <iostream>

static void worker_routine(void *context) {
    //  Socket to talk to dispatcher
    void *receiver = zmq_socket(context, ZMQ_REP);
    zmq_connect(receiver, "inproc://workers");

    while (1) {
        char buffer[1024];
        memset(buffer, 0, 1024);
        zmq_recv(receiver, buffer, 1024, 0);
        std::cout << "Server received request: " << buffer << " address: " << static_cast<void*>(&receiver) << std::endl;
        //  Send reply back to client
        zmq_send(receiver, "World", 5, 0);
    }
    zmq_close(receiver);
}

int main() {
    void *context = zmq_ctx_new();

    //  Socket to talk to clients
    void *clients = zmq_socket(context, ZMQ_ROUTER);
    zmq_bind(clients, "ipc://test");

    //  Socket to talk to workers
    void *workers = zmq_socket(context, ZMQ_DEALER);
    zmq_bind(workers, "inproc://workers");

    //  Launch pool of worker threads
    std::vector<std::thread> thread_pool;
    int thread_nbr;
    for (thread_nbr = 0; thread_nbr < 5; thread_nbr++) {
        thread_pool.emplace_back([context]() {
            worker_routine(context);
        });
    }
    //  Connect work threads to client threads via a queue proxy
    zmq_proxy(clients, workers, NULL);

    //  We never get here, but clean up anyhow
    zmq_close(clients);
    zmq_close(workers);
    zmq_ctx_destroy(context);
    return 0;
}
```

- client.cpp

```cpp
#include <zmq.h>
#include <cstdio>
#include <cstring>

int main() {
    void *context = zmq_ctx_new();

    //  Socket to talk to server
    void *requester = zmq_socket(context, ZMQ_REQ);
    zmq_connect(requester, "ipc://test");

    int request_nbr;
    for (request_nbr = 0; request_nbr != 10; request_nbr++) {
        char strDst[256] = {0};
        snprintf(strDst, 256, "Hello %d", request_nbr);
        zmq_send(requester, strDst, 256, 0);
        char buffer[1024];
        memset(buffer, 0, 1024);
        zmq_recv(requester, buffer, 1024, 0);
        printf("Client received reply %d [%s]\n", request_nbr, buffer);
    }
    zmq_close(requester);
    zmq_ctx_destroy(context);
    return 0;
}
```

#### 参考资料

- [ZeroMQ](https://github.com/zeromq/libzmq)
- [Build instructions](https://github.com/zeromq/cppzmq)
- [ZeroMQ_09 ZMQ多线程编程](https://www.cnblogs.com/vczf/p/12874270.html)
