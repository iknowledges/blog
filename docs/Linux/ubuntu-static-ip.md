# Ubuntu设置静态IP

1. 安装网络工具

```
sudo apt install net-tools
```

2. 输入`ifconfig`查看网卡、IP和子网掩码：

```
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.3.50  netmask 255.255.255.0  broadcast 192.168.3.255
        inet6 fd62:30a1:71de:a900:20c:29ff:fedb:bcf4  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::20c:29ff:fedb:bcf4  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:db:bc:f4  txqueuelen 1000  (Ethernet)
        RX packets 7031  bytes 7951164 (7.9 MB)
        RX errors 0  dropped 458  overruns 0  frame 0
        TX packets 3122  bytes 339012 (339.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

3. 输入`route -n`查看网关：

```
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.3.1     0.0.0.0         UG    100    0        0 ens33
192.168.3.0     0.0.0.0         255.255.255.0   U     100    0        0 ens33
192.168.3.1     0.0.0.0         255.255.255.255 UH    100    0        0 ens33
```

4. 修改配置文件`/etc/netplan/00-installer-config.yaml`如下：

```yaml
network:
  ethernets:
    ens33:
      dhcp4: false
      addresses: [192.168.3.50/24]
      gateway4: 192.168.3.1
      nameservers:
        addresses: [8.8.8.8, 114.114.114.114]
  version: 2
  renderer: networkd
```

- ens33: 网卡名称
- addresses: 静态ip和掩码，/24也就是255.255.255.0
- gateway4: 网关
- nameservers: DNS服务器
- renderer: 如果以服务器模式安装Ubuntu，则使用networkd作为渲染器

5. 使配置生效

```
sudo netplan apply
```

#### 参考资源

- [Ubuntu设置静态IP地址的几种方法](https://blog.csdn.net/fun_tion/article/details/126750615)
