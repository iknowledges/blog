# Hyper-V设置固定IP并连接外网

## Hyper-V管理器

1. 新建虚拟交换机，名称填写：NAT，连接类型选择【内部网络】。
2. 打开虚拟机设置，【网络适配器】中虚拟交换机选择刚创建的NAT。

## 网络连接

#### 虚拟网卡

1. 打开【控制面板】->【网络和Internet】->【网络连接】，找到刚刚创建的虚拟网卡，如vEthernet(NAT)。
2. 右键该网卡，选择【属性】->【网络】-【Internet协议版本4(TCP/IPv4)】，再点击属性。
3. 设置IP地址：192.168.137.1，子网掩码：255.255.255.0，其他不填，点击【确定】。

#### 虚拟网卡访问外网

1. 在【控制面板】->【网络和Internet】->【网络连接】找到宿主机正常联网的网卡。
2. 右键该网卡，选择【属性】->【共享】。
3. 勾选【允许其他网络用户通过此计算机的Internet连接来连接】，然后下拉菜单中选择前面创建的虚拟网卡vEthernet(NAT)
4. 如果虚拟机配置静态IP后连外网不成功，可将网卡共享关闭然后重新设置。

## Netplan设置静态IP（Ubuntu）

1. 修改配置文件

```
vim /etc/netplan/50-cloud-init.yaml
```

修改内容如下：

```yaml
network:
    ethernets:
        eth0:
            dhcp4: false
            addresses: [192.168.137.10/8]
            optional: true
            gateway4: 192.168.137.1
            nameservers:
              addresses: [192.168.137.1]
    version: 2
```

2. 重启虚拟机查看eth0网卡设置是否生效

```
ip addr
```

## Systemd Networkd服务设置静态IP（ArchLinux推荐）

1. 编辑创建一个网络配置文件，下面命令中的eth0要与你连接的网卡名称一致

```
vim /etc/systemd/network/eth0.network
```

内容如下：

```
[Match]
Name=eth0

[Network]
Address=192.168.137.10/24
Gateway=192.168.137.1
DNS=192.168.137.1
```

2. 启动systemd-networkd服务和systemd-resolved（需要这个服务来解析DNS）

```
systemctl enable systemd-networkd
systemctl start systemd-networkd

systemctl enable systemd-resolved
systemctl start systemd-resolved
```

## NetworkManager设置静态IP

1. 查看网卡信息

```
# 查看网卡连接状态，结果显示eth0的网卡已连接
nmcli device status
# 查看网卡对应的UUID
nmcli connection show
```

2. 配置静态IP

```
nmcli connection modify 12345678-1234-1234-1234-1234567890ab ipv4.addresses 192.168.137.10/24 ipv4.gateway 192.168.137.1 ipv4.dns 192.168.137.1 ipv4.method manual
```

3. 重启网络

```
nmcli connection down 12345678-1234-1234-1234-1234567890ab
nmcli connection up 12345678-1234-1234-1234-1234567890ab
```

#### 参考资料

- [Hyper-V虚拟机配置内部网络固定IP并连接外网](https://blog.csdn.net/vincent_duan/article/details/120339781)
- [Hyper-v虚拟机固定ip](https://zhuanlan.zhihu.com/p/364808542)
- [Hyper-V虚拟机没有网络怎么解决？](https://www.abackup.com/enterprise-backup/hyper-v-vm-no-internet-666.html)
- [arch linux配置IP(静态或动态)](https://cloud-atlas.readthedocs.io/zh-cn/latest/linux/arch_linux/archlinux_config_ip.html)
- [如何使用NetworkManager配置静态地址](https://www.sddts.cn/index.php/archives/1311/)
