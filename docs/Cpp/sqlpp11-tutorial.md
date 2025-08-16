# Sqlite3中Sqlpp11的使用教程

## 根据SQL生成C++源码文件

1. 安装python依赖

```
pip install pyparsing
```

2. 编写建表SQL语句，并保存为student.sql文件：

```sql
CREATE TABLE student (
    id INTEGER PRIMARY KEY,
    realname TEXT,
    age INT
)
```

3. 执行下面命令将sql转换成C++头文件：

```
python path/to/sqlpp11-ddl2cpp student.sql ./Student TestDb
```

- 其中Student是生成目标文件的名称，TestDb是命名空间。

## 数据库操作

1. 创建表格

```cpp
#include <sqlpp11/sqlite3/sqlite3.h>
#include <sqlpp11/sqlpp11.h>
#include <fstream>

namespace sql = sqlpp::sqlite3;

void createTable() {
    std::ifstream file("../student.sql");
    if (!file.is_open()) {
        return;
    }
    std::stringstream buffer;
    buffer << file.rdbuf();

    sql::connection_config config;
    config.path_to_database = "test.db";
    config.flags = SQLITE_OPEN_READWRITE | SQLITE_OPEN_CREATE;
    config.debug = true;

    sql::connection db(config);

    db.execute(buffer.str());
}
```

2. 增删改查

```cpp
sql::connection_config config;
config.path_to_database = "test.db";
config.flags = SQLITE_OPEN_READWRITE | SQLITE_OPEN_CREATE;
config.debug = debug;

sql::connection db(config);

// insert
TestDb::Student student{};
db(insert_into(student).set(student.realname = "zhangsan", student.age = 10));
std::cout << "Last Insert ID: " << db.last_insert_id() << "\n";

// update
db(update(student).set(student.realname = "lisi").where(student.id.in(1)));

// query
auto result = db(select(student.realname, student.age).from(student).where(student.age > 1));
for (const auto &row: result) {
    std::cout << row.realname << ":" << row.age << std::endl;
}

// remove
db(remove_from(student).where(student.id == 2));
```

## Datetime类型的使用示例

```cpp
#include <iostream>
#include "Sample.h"
#include "MockDb.h"
#include <sqlpp11/sqlpp11.h>

SQLPP_ALIAS_PROVIDER(now)

#if _MSC_FULL_VER >= 190023918
// MSVC Update 2 provides floor, ceil, round, abs in chrono (which is C++17 only...)
using ::std::chrono::floor;
#else
using ::date::floor;
#endif

int DateTime(int, char* [])
{
  MockDb db = {};
  MockDb::_serializer_context_t printer = {};
  const auto t = test::TabDateTime{};

  for (const auto& row : db(select(::sqlpp::value(std::chrono::system_clock::now()).as(now))))
  {
    std::cout << row.now;
  }
  for (const auto& row : db(select(all_of(t)).from(t).unconditionally()))
  {
    std::cout << row.colDayPoint;
    std::cout << row.colTimePoint;
  }
  printer.reset();
  std::cerr << serialize(::sqlpp::value(std::chrono::system_clock::now()), printer).str() << std::endl;

  db(insert_into(t).set(t.colDayPoint = floor<::sqlpp::chrono::days>(std::chrono::system_clock::now())));
  db(insert_into(t).set(t.colTimePoint = floor<::sqlpp::chrono::days>(std::chrono::system_clock::now())));
  db(insert_into(t).set(t.colTimePoint = std::chrono::system_clock::now()));
  db(insert_into(t).set(t.colTimeOfDay = ::sqlpp::chrono::time_of_day(std::chrono::system_clock::now())));

  db(update(t)
         .set(t.colDayPoint = floor<::sqlpp::chrono::days>(std::chrono::system_clock::now()))
         .where(t.colDayPoint < std::chrono::system_clock::now()));
  db(update(t)
         .set(t.colTimePoint = floor<::sqlpp::chrono::days>(std::chrono::system_clock::now()),
              t.colTimeOfDay = ::sqlpp::chrono::time_of_day(std::chrono::system_clock::now()))
         .where(t.colDayPoint < std::chrono::system_clock::now()));
  db(update(t)
         .set(t.colTimePoint = std::chrono::system_clock::now(),
              t.colTimeOfDay = ::sqlpp::chrono::time_of_day(std::chrono::system_clock::now()))
         .where(t.colDayPoint < std::chrono::system_clock::now()));

  db(remove_from(t).where(t.colDayPoint == floor<::sqlpp::chrono::days>(std::chrono::system_clock::now())));
  db(remove_from(t).where(t.colDayPoint == std::chrono::system_clock::now()));
  db(remove_from(t).where(t.colTimePoint == floor<::sqlpp::chrono::days>(std::chrono::system_clock::now())));
  db(remove_from(t).where(t.colTimePoint == std::chrono::system_clock::now()));
  db(remove_from(t).where(t.colTimeOfDay == ::sqlpp::chrono::time_of_day(std::chrono::system_clock::now())));

  return 0;
}
```

#### 参考资料

- [C++ ORM框架:SQLPP11教程](https://www.xnip.cn/ruanjian/anli/125902.html)
- [Sqlpp11 Hunterized](https://hunter.readthedocs.io/en/latest/packages/pkg/Sqlpp11.html)
