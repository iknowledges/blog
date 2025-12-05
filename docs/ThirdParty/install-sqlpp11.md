# Sqlpp11源码编译安装教程

## Windows下安装

1. 下载[sqlpp11-0.64.zip](https://github.com/rbock/sqlpp11/tags)源码并解压。

2. 打开cmake-gui进行如下设置，并依次点击Configure和Generate

> SQLite3

- Where is the source code: E:/third_party_build/sqlpp11-0.64
- Where to build the binaries: E:/third_party_build/sqlpp11-0.64/build_win
- CMAKE_INSTALL_PREFIX: E:/third_party/sqlpp11
- Boost_ROOT: E:/third_party/Boost
- BUILD_SQLITE3_CONNECTOR: ON
- SQLite3_ROOT: E:/third_party/SQLite3

> PostgreSQL

- Where is the source code: E:/third_party_build/sqlpp11-0.64
- Where to build the binaries: E:/third_party_build/sqlpp11-0.64/build_win
- CMAKE_INSTALL_PREFIX: E:/third_party/sqlpp11
- BUILD_POSTGRESQL_CONNECTOR: ON
- PostgreSQL_ROOT: E:/Software/PostgreSQL/16

3. 用Visual Studio打开build目录下生成的sqlpp11.sln工程，右键ALL_BUILD点击生成，然后右键INSTALL点击生成。

## Ubuntu下安装

1. 进入sqlpp11的源代码目录执行下面命令：

```
ccmake -S . -B ./build_linux -DBoost_ROOT=/path/to/third_party_linux/Boost -DSQLite3_ROOT=/path/to/third_party_linux/SQLite3
```

2. 输入c，修改完配置，再输入g：

- CMAKE_INSTALL_PREFIX: /path/to/third_party_linux/sqlpp11
- BUILD_SQLITE3_CONNECTOR: ON

3. 编译并安装：

```
cmake --build ./build_linux
cmake --install ./build_linux
```

## SQLite3中的CMakeList.txt

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR} ${THIRD_PARTY_DIR}/sqlpp11)

# SQLite3
find_package(SQLite3 CONFIG REQUIRED)

# Howard Hinnant’s date library
find_package(date CONFIG REQUIRED)
message(STATUS "Using date ${date_VERSION}")

# Sqlpp11
find_package(Sqlpp11 CONFIG REQUIRED)
message(STATUS "Using Sqlpp11 ${Sqlpp11_VERSION}")

target_link_libraries(${PROJECT_NAME} PRIVATE sqlpp11::sqlpp11 sqlpp11::sqlite3)
```

编写测试代码main.cpp

```cpp
#include <sqlpp11/sqlite3/sqlite3.h>

namespace sql = sqlpp::sqlite3;

int main()
{
    sql::connection_config config;
    config.path_to_database = "test.db";
    config.flags = SQLITE_OPEN_READWRITE | SQLITE_OPEN_CREATE;
    config.debug = true;

    sql::connection db(config);

    db.execute(R"(CREATE TABLE tab_foo (
		omega bigint(20) DEFAULT NULL
			))");

    return 0;
}
```

## PostgreSQL中的CMakeList.txt

```
list(APPEND CMAKE_PREFIX_PATH ${THIRD_PARTY_DIR} ${THIRD_PARTY_DIR}/sqlpp11)

# PostgreSQL
set(PostgreSQL_ROOT "E:/Software/PostgreSQL/16")
find_package(PostgreSQL REQUIRED)

# Howard Hinnant’s date library
find_package(date CONFIG REQUIRED)
message(STATUS "Using date ${date_VERSION}")

# Sqlpp11
find_package(Sqlpp11 CONFIG REQUIRED)
message(STATUS "Using Sqlpp11 ${Sqlpp11_VERSION}")

target_link_libraries(${PROJECT_NAME} PRIVATE sqlpp11::sqlpp11 sqlpp11::postgresql)
```

编写测试代码main.cpp

```cpp
#include <sqlpp11/sqlpp11.h>
#include <sqlpp11/postgresql/connection.h>

namespace sql = sqlpp::postgresql;

int main() {
	auto config = std::make_shared<sql::connection_config>();
	config->host = "localhost";
	config->user = "postgres";
	config->password = "postgres";
	try {
		sql::connection db(config);
		std::cout << "Connected success!" << std::endl;
	} catch (const sqlpp::postgresql::broken_connection& ex) {
		std::cout << "Got exception: '" << ex.what() << "'" << std::endl;
		return -1;
	}
	return 0;
}
```

## MySQL中的CMakeList.txt

```
# PostgreSQL
find_package(libmysqlclient REQUIRED)
message(STATUS "Using libmysqlclient ${libmysqlclient_VERSION}")

# Sqlpp11
find_package(Sqlpp11 CONFIG REQUIRED)
message(STATUS "Using Sqlpp11 ${Sqlpp11_VERSION}")

target_link_libraries(${PROJECT_NAME} PRIVATE libmysqlclient::libmysqlclient sqlpp11::sqlpp11)
```

#### 参考资料

- [sqlpp11](https://github.com/rbock/sqlpp11)
