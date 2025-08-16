# WSL配置防火墙规则

1. 以管理员身份打开powershell，输入ipconfig查看网络信息：

```
以太网适配器 vEthernet (WSL (Hyper-V firewall)):

   连接特定的 DNS 后缀 . . . . . . . :
   本地链接 IPv6 地址. . . . . . . . : fe80::e3d0:9267:45b9:b3b%47
   IPv4 地址 . . . . . . . . . . . . : 172.22.128.1
   子网掩码  . . . . . . . . . . . . : 255.255.240.0
   默认网关. . . . . . . . . . . . . :
```

2. 新建防火墙规则，InterfaceAlias要和上面查到的名称一致：

```powershell
New-NetFirewallRule -DisplayName "WSL" -Direction Inbound  -InterfaceAlias "vEthernet (WSL (Hyper-V firewall))"  -Action Allow
```

#### 参考资料

- [WSL2 连接 Windows 防火墙问题解决方案](https://zhuanlan.zhihu.com/p/365058237)