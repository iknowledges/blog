# Sqlite3使用教程

## Ubuntu中安装sqlite3

```
sudo apt install sqlite3
```

## 常用命令

```
# 打开数据库文件
sqlite3 test.db
# 以显示标题，column模式，打开数据库
sqlite3 -header -column test.db
# 查看数据库信息
.databases
# 查看表格
.tables
# 显示标题
.header on
# 输出格式包括：ascii box column csv html insert json line list markdown qbox quote table tabs tcl
.mode column
# 退出
.quit
# 加载sql脚本
.read script.sql
# 命令行中直接执行sql脚本
sqlite3 test.db < script.sql
```

#### 参考资料

- [如何打开和使用SQLite文件](https://cn.linux-console.net/?p=37498)
- [SQL 在命令行中运行Sqlite3脚本](https://geek-docs.com/sql/sql-ask-answer/635_sql_running_a_sqlite3_script_from_command_line.html)
