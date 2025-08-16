# CentOS7管理防火墙

## 防火墙服务

```
systemctl enable firewalld
systemctl disable firewalld
systemctl start firewalld
systemctl stop firewalld
systemctl status firewalld
```

## firewall-cmd命令

```
# 查看防火墙状态
firewall-cmd --state
# 重新加载配置
firewall-cmd --reload
# 查看开放的端口
firewall-cmd --list-ports
# 开启防火墙端口，添加后必须重新加载配置
firewall-cmd --zone=public --add-port=9200/tcp --permanent
# 关闭防火墙端口
firewall-cmd --zone=public --remove-port=9200/tcp --permanent
```

[centOS7 查看防火墙状态 及 防火墙配置](https://blog.csdn.net/ciel_yu/article/details/118000103)
