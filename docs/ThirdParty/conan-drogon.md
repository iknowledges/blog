# Conan安装Drogon教程

1. 使用conan inspect命令查看编译Drogon的options选项，inspect指定的路径下必须有设置好这些编译选项conanfile.py文件：

```
conan inspect ~/.conan2/p/drogo7178f7ce738fb/e/
```

注意：Drogon的源代码位于~/.conan2/p/drogo7178f7ce738fb/s/src/下，后面解决问题就是修改这个目录下的源代码。

2. 修改自己项目下的conanfile.py：

```python
import os
from conan import ConanFile
from conan.tools.cmake import CMakeToolchain, CMakeDeps
from conan.tools.env import VirtualRunEnv


class DrogonRecipe(ConanFile):
    settings = "os", "compiler", "build_type", "arch"

    def requirements(self):
        self.requires("drogon/1.9.11", options={"with_mysql": True, "with_yaml_cpp": True, "with_boost": False})
    
    def generate(self):
        # 获取drogon_ctl所在目录
        drogon_pkg = self.dependencies["drogon"]
        drogon_bin_path = os.path.join(drogon_pkg.package_folder, "bin")
        # 将drogon_ctl添加到环境变量PATH
        ms = VirtualRunEnv(self)
        env = ms.environment()
        env.append_path("PATH", drogon_bin_path)
        ms.generate()

        deps = CMakeDeps(self)
        deps.generate()
        tc = CMakeToolchain(self)
        tc.generate()
```

3. 编译安装依赖

```
conan install . --output-folder=cmake-build-debug/ --profile=debug --build=drogon/1.9.11
```

4. 使用drogon_ctl

```
source cmake-build-debug/conanrun.sh
drogon_ctl version
source cmake-build-debug/deactivate_conanrun.sh
```

## 问题解决

1. Drogon连接MySQL时SSL验证失败

```
ERROR Error(2026) "TLS/SSL error: Certificate verification failure: The certificate is NOT trusted." - MysqlConnection.cc:333
ERROR Failed to mysql_real_connect() - MysqlConnection.cc:335
```

参考[在windows下使用drogon_ctl create model models生成mysql数据库模型类报SSL错误](https://github.com/drogonframework/drogon/issues/2311)，修改orm_lib/src/mysql_impl/MysqlConnection.cc如下：

```cpp
MysqlConnection::MysqlConnection(trantor::EventLoop *loop,
                                 const std::string &connInfo)
    : DbConnection(loop),
      mysqlPtr_(std::shared_ptr<MYSQL>(new MYSQL, [](MYSQL *p) {
          mysql_close(p);
          delete p;
      }))
{
    static MysqlEnv env;
    static thread_local MysqlThreadEnv threadEnv;
    mysql_init(mysqlPtr_.get());
    mysql_options(mysqlPtr_.get(), MYSQL_OPT_NONBLOCK, nullptr);
    // ssl set start
    mysql_ssl_set(mysqlPtr_.get(), nullptr, nullptr, nullptr, nullptr, nullptr);
    my_bool verify = 0;
    mysql_options(mysqlPtr_.get(), MYSQL_OPT_SSL_VERIFY_SERVER_CERT, (void *)&verify);
    mysql_options(mysqlPtr_.get(), MYSQL_OPT_SSL_ENFORCE, (void *)&verify);
    // ssl set end
#ifdef HAS_MYSQL_OPTIONSV
    mysql_optionsv(mysqlPtr_.get(), MYSQL_OPT_RECONNECT, &reconnect_);
#endif
    // Get the key and value
    auto connParams = parseConnString(connInfo);
    for (auto const &kv : connParams)
    {
        auto key = kv.first;
        auto value = kv.second;

        std::transform(key.begin(),
                       key.end(),
                       key.begin(),
                       [](unsigned char c) { return tolower(c); });
        // LOG_TRACE << key << "=" << value;
        if (key == "host")
        {
            host_ = value;
        }
        else if (key == "user")
        {
            user_ = value;
        }
        else if (key == "dbname")
        {
            // LOG_DEBUG << "database:[" << value << "]";
            dbname_ = value;
        }
        else if (key == "port")
        {
            port_ = value;
        }
        else if (key == "password")
        {
            passwd_ = value;
        }
        else if (key == "client_encoding")
        {
            characterSet_ = value;
        }
    }
}
```

2. 添加yaml支持后，编译时报`yaml-cpp not used`，需要修改源代码中的CMakeLists.txt，将YAML_CPP_LIBRARIES修改成yaml-cpp_LIBRARIES：

```
# yamlcpp
if(BUILD_YAML_CONFIG)
    find_package(yaml-cpp QUIET)
    if(yaml-cpp_FOUND)
        if (NOT yaml-cpp_LIBRARIES)
            find_library(YAML_CPP_LINK_LIBRARY "yaml-cpp")
            if(NOT YAML_CPP_LINK_LIBRARY)
                message(STATUS "yaml-cpp not used")
            else()
                message(STATUS "yaml-cpp found ")
                target_link_libraries(${PROJECT_NAME} PUBLIC ${YAML_CPP_LINK_LIBRARY})
                target_compile_definitions(${PROJECT_NAME} PUBLIC HAS_YAML_CPP)
            endif()
        else()
            message(STATUS "yaml-cpp found ")
            target_link_libraries(${PROJECT_NAME} PUBLIC ${yaml-cpp_LIBRARIES})
            target_compile_definitions(${PROJECT_NAME} PUBLIC HAS_YAML_CPP)
        endif()
    else()
        message(STATUS "yaml-cpp not used")
    endif()
else()
    message(STATUS "yaml-cpp not used")
endif(BUILD_YAML_CONFIG)
```