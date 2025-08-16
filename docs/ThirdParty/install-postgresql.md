# PostgreSQL安装教程

## Windows

1. 在官网下载[postgresql-16.9-3-windows-x64.exe](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)，然后执行安装程序。
2. 结尾取消安装Stack Builder，然后点击Finish。
3. PostgreSQL安装时默认不会修改系统环境变量，所以要将安装目录下的bin加入到环境变量Path中，然后在命令行输入下面命令进行测试：

```
pg_config --version
```

## CMakeList.txt中使用libpg

```
# PostgreSQL
set(PostgreSQL_ROOT "E:/Software/PostgreSQL/16")
find_package(PostgreSQL REQUIRED)

target_link_libraries(${PROJECT_NAME} PRIVATE PostgreSQL::PostgreSQL)
```

编写测试main.cpp

```cpp
#include <iostream>
#include <libpq-fe.h>


int main()
{
	const char* conninfo = "postgresql://postgres@localhost/postgres?password=password&connect_timeout=10";
	PGconn* conn = PQconnectdb(conninfo);
	if (PQstatus(conn) != CONNECTION_OK) {
		std::cerr << "Connect Failed: " << PQerrorMessage(conn) << std::endl;
	} else {
		std::cout << "Hello PostgreSQL" << std::endl;
	}
	return 0;
}
```

#### 参考资料

- [Installing PostgreSQL](https://www.enterprisedb.com/docs/supported-open-source/postgresql/installing/)
