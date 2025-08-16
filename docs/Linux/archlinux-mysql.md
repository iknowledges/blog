# ArchLinux安装MySQL

1. 安装MySQL

```
sudo pacman -S mysql
```

2. 初始化MySQL，basedir是MySQL的安装路径，datadir是MySQL的数据存储路径

```
sudo mysqld --initialize --user=mysql --basedir=/usr --datadir=/var/lib/mysql
```

注：如果初始化报错，可以`sudo rm -rf /var/lib/mysql`删除数据文件再重新初始化。

3. 启动MySQL

```
sudo systemctl start mysqld
sudo systemctl status mysqld
```

4. 使用初始化生成的密码登录MySQL

```
mysql -u root -p
```

5. 修改登录密码

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'root-password';
```

6. 修改远程登录权限

```
use mysql;
update user set host='%' where user='root';
grant all privileges on *.* to 'root'@'%';
flush privileges;
```

#### 其他问题

- 远程连接报错：2013 - Lost connection to MySQL server at 'waiting for initial communication packet', system error: 0 "Internal error/check (Not system error)"

1. 修改配置文件，跳过DNS反向解析

```
vim /etc/mysql/my.cnf
```

内容如下：

```
[mysqld]
skip-name-resolve
```

2. 重启服务

```
systemctl restart mysqld
```

#### 参考资料

- [Arch Linux 安装 MySQL 8.0](https://blog.csdn.net/qq_40829735/article/details/81166669)
- [2.9.1 Initializing the Data Directory](https://dev.mysql.com/doc/refman/8.4/en/data-directory-initialization.html)
