# Sqlite3使用教程

## Ubuntu中安装sqlite3

```
sudo apt install sqlite3
```

## 常用命令

```
# 打开数据库文件
sqlite3 test.db
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
```

#### 参考资料

- [如何打开和使用SQLite文件](https://cn.linux-console.net/?p=37498)