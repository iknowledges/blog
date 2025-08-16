# Ubuntu创建新用户

1. 创建用户目录
```bash
useradd -r -m -s /bin/bash hello
```
- -r：建立系统账号
- -m：自动创建home下面的主目录
- -s：指定用户登入后所使用的shell

2. 修改用户权限
```bash
chmod +w /etc/sudoers
vim /etc/sudoers
# 按下面操作修改
chmod -w /etc/sudoers
# 设置密码
passwd hello
```
# 在/etc/sudoers添加：
```
# User privilege specification
hello ALL=(ALL:ALL) ALL
```
- 第一段：用户名或者用户组，这里是hello用户；
- 第二段：表示来源地，即从哪执行这条命令。ALL表示所有计算机；
- 第三段：表示sudo可以切换到什么用户。ALL表示所有用户；
- 第四段：表示sudo可以切换到哪些组下的用户。ALL表示所有组；
- 第五段：表示sudo之后能够执行的命令。ALL表示执行命令都需要密码；NOPASSWD:ALL表示执行任意命令都不需要密码。

在docker中使用指定用户登陆容器
```bash
sudo docker exec -it --user=hello myubt bash
```

3. 删除用户
```bash
sudo userdel hello
sudo rm -rf /home/hello
```
删除/etc/sudoers中用户hello的配置

## 参考链接
[Ubuntu创建新用户的正确姿势](https://www.cnblogs.com/geyouneihan/p/9839153.html)
