# CentOS7不显示IP的解决方法

原因：默认网卡没有启动。

1. 修改配置

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

将`onboot=no`改为`onboot=yes`

2. 重启网络

```
service network restart
ifconfig
```

[解决centos-7ifconfig没有IP地址](https://blog.csdn.net/zhaohangbo/article/details/106025932)
