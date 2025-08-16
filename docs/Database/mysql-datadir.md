# MySQL迁移data文件夹

1. 首先我们需要把mysql文件拷贝到其他空间大的磁盘上，默认文件路径是/var/lib/mysql

```bash
mv /var/lib/mysql  /home
```

2. 其次修改配置文件/etc/my.cnf

```
# datadir=/var/lib/mysql
datadir=/home/mysql
# socket=/var/lib/mysql/mysql.sock
socket=/home/mysql/mysql.sock
```

3. 然后创建一个软链接

```bash
ln -s /home/mysql  /var/lib/mysql
```

4. 给新的目录权限

```bash
chown -R mysql.mysql /home/mysql/
```

5. 最后重启数据库

```bash
service mysqld restart
```

### 参考资源

[mysql更改datadir](https://blog.csdn.net/qq_40006446/article/details/88846693)
