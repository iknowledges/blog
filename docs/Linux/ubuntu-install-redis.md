# Ubunut安装Redis

## 一、安装redis服务器

```
sudo apt install redis-server
```

## 二、基本操作

```
# 启动
systemctl start redis-server
# 停止
systemctl stop redis-server
# 查看状态
systemctl status redis-server
# 打开客户端
reids-cli
```
## 三、修改配置

```
vim /etc/redis/redis.conf
```

修改内容如下：

```
# bind 127.0.0.1 ::1
bind 0.0.0.0
# protected-mode yes
protected-mode no
```

设置密码，默认用户名是default：

```
# requirepass foobared
requirepass 123456
```

如果想创建其他用户，可以使用ACL命令。例如，创建一个名为myuser的用户并设置密码：

```
redis-cli
# 添加用户
ACL SETUSER myuser on >mypassword ~* +@all
# 删除用户
ACL DELUSER myuser
```

- myuser: 用户名
- on: 用户开启状态
- >mypassword: 设置密码为mypassword
- ~*: 用户可以访问所有键
- +@all: 用户拥有所有权限

#### 参考资料

- [redis7设置用户和密码](https://blog.51cto.com/u_16213421/11904725)
