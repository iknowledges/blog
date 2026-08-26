# SQLx教程

## 安装SQLx CLI

```
# only for postgres
cargo install sqlx-cli --no-default-features --features native-tls,postgres
```

## 迁移数据库

1. 在当前目录创建`.env`文件：

```
DATABASE_URL=postgres://postgres@localhost/my_database
```

2. 生成sql文件，并迁移到数据库：

```
# create a migration sql file
sqlx migrate add create_table
# run migrations
sqlx migrate run
```

## offline模式编译项目

使用macros编写sql时，offline模式可以跳过编译时的sql检查：

```
cargo sqlx prepare --workspace
SQLX_OFFLINE=true cargo build --release
```

#### 参考资料

- [SQLx CLI](https://docs.rs/crate/sqlx-cli/latest)
