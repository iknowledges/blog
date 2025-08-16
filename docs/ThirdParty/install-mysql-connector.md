# Ubuntu编译MySQL Connector/C++

1. 安装openssl

```
sudo apt install libssl-dev
openssl version
```

2. 克隆项目

```
git clone -b 8.4.0 https://github.com/mysql/mysql-connector-cpp.git
cd mysql-connector-cpp
```

3. 开始编译

```
mkdir build
cd build
# Configuring Connector/C++
# 使用WITH_JDBC和MYSQL_DIR要先编译MySQL
cmake -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR/mysql-concpp -DCMAKE_BUILD_TYPE=Debug -DWITH_JDBC=ON -DMYSQL_DIR=$MY_INSTALL_DIR/mysql-server ..
# Building Connector/C++
cmake --build . --config Debug
# Installing Connector/C++
cmake --build . --target install --config Debug
```

4. CMakeLists.txt中使用

```
list(APPEND CMAKE_PREFIX_PATH $ENV{MY_INSTALL_DIR})

find_package(mysql-concpp CONFIG REQUIRED)
include_directories(${MYSQL_CONCPP_INCLUDE_DIR})

target_link_libraries(${PROJECT_NAME} PRIVATE mysql::concpp mysql::concpp-jdbc)
```

#### 问题处理

```
Could NOT find MySQL Connector/C++ at
  /mnt/f/third_party_install/mysql-concpp/lib64.  (missing:
  MYSQL_CONCPP_FOUND) (found version "8.4.0")
```

- 解决方法：将lib64/debug下的库文件拷贝到lib64目录下

#### 参考资料

- [Chapter 4 Installing Connector/C++ from Source](https://dev.mysql.com/doc/connector-cpp/8.4/en/connector-cpp-installation-source.html)
- [Using Connector/C++ 8](https://dev.mysql.com/doc/dev/connector-cpp/latest/usage.html)
