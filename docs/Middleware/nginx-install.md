# nginx安装和常用命令

## 源码包安装

1. 下载安装包并解压，地址：https://nginx.org/en/download.html

```
tar -zxvf nginx-1.18.0.tar.gz
cd nginx-1.18.0
```

2. 使用默认配置安装

```
./configure
make
make install
# 查看安装路径
whereis nginx
```

## Ubuntu安装

官方教程：https://nginx.org/en/linux_packages.html#Ubuntu

```
sudo apt update
sudo apt install nginx
```

## Nginx常用命令

```
cd /usr/local/nginx/sbin/
# 启动
./nginx
# 停止
./nginx -s stop
# 安全退出
./nginx -s quit
# 重新加载配置文件
./nginx -s reload
# 查看nginx进程
ps aux|grep nginx
# Windows下终止进程，/f是强制终止，/t终止指定的进程及其子进程，/im指定进程名称
taskkill /f /t /im nginx.exe
```

- 防火墙相关命令：

```
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop
# 查看防火墙规则
firewall-cmd --list-all
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload
# 参数解释
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```

#### 参考资料

- [Nginx快速入门](https://www.kuangstudy.com/bbs/1353634800149213186)
