# PostgreSQL安装教程

## 一、安装PostgreSQL

```
sudo apt install postgresql
# 启动服务
sudo systemctl start postgresql
# 停止服务
sudo systemctl stop postgresql
# 重启服务
sudo systemctl restart postgresql
# 查看服务状态
sudo systemctl status postgresql
# 命令行连接PostgreSQL
psql -h localhost -p 5432 -U postgres -d postgres
```

## 二、设置远程连接

1. 修改postgresql.conf

```
sudo vim /etc/postgresql/16/main/postgresql.conf
```

配置如下：

```
# 修改监听地址，默认为localhost
listen_addresses = '*'
# 修改端口号
port = 5432
```

2. 修改pg_hba.conf

```
sudo vim /etc/postgresql/16/main/pg_hba.conf
```

配置如下：

```
# TYPE  DATABASE  USER  ADDRESS         METHOD
host    all       all   192.168.0.1/32  md5
```

## 三、设置PostgreSQL默认用户密码

```
# 进入PostgreSQL命令行
sudo -u postgres psql
# 修改postgres用户密码
ALTER USER postgres WITH ENCRYPTED PASSWORD '123456';
# 退出然后重启服务
\q
```

## 四、创建新用户

1. 切换到postgres用户

```
sudo su - postgres
```

2. 创建新用户

```
$ createuser --interactive
Enter name of role to add: admin
Shall the new role be a superuser? (y/n) n
Shall the new role be allowed to create databases? (y/n) n
Shall the new role be allowed to create more new roles? (y/n) n
```

3. 给新用户设置密码

```
# 进入PostgreSQL命令行
psql
# 修改admin用户密码
ALTER USER admin WITH ENCRYPTED PASSWORD '123456';
# 退出然后重启服务
\q
```

#### 参考资料

- [在 Ubuntu 24.04 上安装和配置 PostgreSQL 教程](https://zhuanlan.zhihu.com/p/1900465497427911910)