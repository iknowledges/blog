# Ubunut安装MySQL

## 一、安装mysql

### 安装MySQL5.7

1. 查询mysql版本信息

```shell
# 使用apt-cache madison列出软件的所有来源
apt-cache madison mysql-server
# 使用apt-cache policy列出软件的所有来源
apt-cache policy mysql-server
# 使用apt-cache showpkg列出软件的所有来源
apt-cache showpkg mysql-server
```
2. 通过apt-get安装指定版本软件

```shell
sudo apt-get install mysql-server=5.7.11-0ubuntu6
sudo apt-get install mysql-client=5.7.11-0ubuntu6
sudo apt-get install libmysqlclient=5.7.11-0ubuntu6
```
3. 以上3个软件包安装完成后，使用如下命令查询是否安装成功：
```shell
sudo netstat -tap | grep mysql
```
查询结果如下图所示，表示安装成功。
```
bryant@bryant-virtual-machine:~$ sudo netstat -tap | grep mysql
tcp   0   0   localhost:mysql   *:*   LISTEN   13918/mysqld 
```

### 安装MySQL8.0

```
sudo apt-get install mysql-server=8.0.40-0ubuntu0.24.04.1
```

## 二、设置初始密码

1. 没有设置密码前使用root身份直接进入mysql：

```shell
sudo su
mysql
```

2. 设置用户密码

- MySQL5.7

```sql
# 查看用户信息
select user, plugin from mysql.user;
# 修改密码
update mysql.user set authentication_string=PASSWORD('123456'), plugin='mysql_native_password' where user='root';
flush privileges;
```

- MySQL8.0

```sql
# 置空字段
update mysql.user set authentication_string='', plugin='mysql_native_password' where user='root';
# 修改密码
alter user 'root'@'localhost' IDENTIFIED BY '123456';
flush privileges;
```

3. 退出重启

```shell
sudo /etc/init.d/mysql restart
```

## 三、设置mysql远程访问

1. 编辑mysql配置文件，把其中bind-address = 127.0.0.1注释掉

```shell
vim /etc/mysql/mysql.conf.d/mysqld.cnf 
```

2. 使用root进入mysql命令行，执行如下命令，%代表所有远程ip都可以访问

- MySQL5.7

```sql
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
flush privileges;
```

- MySQL8.0

```
update mysql.user set host='%' where user='root';
grant all privileges on *.* to 'root'@'%';
flush privileges;
```

3. 重启mysql

```shell
sudo /etc/init.d/mysql restart
```
