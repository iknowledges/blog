# WSL2配置端口转发

## 防火墙设置

1. 依次打开【控制面板】->【系统和安全】->【Windows Defender防火墙】->【高级设置】
2. 依次点击【入栈规则】->【新建规则】->【端口】->【下一页】
3. 选择【TCP】和【特定本地端口】，输入8000，点击【下一页】
4. 选择【允许连接】，点击【下一页】，默认全选【域】、【专用】和【公用】，点击【下一页】
5. 【名称】输入WSL 8000，点击【完成】

## 端口转发

以管理员身份打开PowerShell

```
# 添加端口转发配置
netsh interface portproxy add v4tov4 listenport=8000 listenaddress=0.0.0.0 connectport=8000 connectaddress=172.20.188.118
# 显示配置
netsh interface portproxy show all
# 删除端口转发配置
netsh interface portproxy delete v4tov4 listenport=8000 listenaddress=0.0.0.0
```

- listenport：表示要监听的Windows端口。
- listenaddress：表示监听地址，0.0.0.0表示匹配所有地址，比如Windows既有Wifi网卡又有有线网卡，那么访问任意两个网卡都会被监听到，当然也可以指定其中之一的IP的地址。
- connectaddress：要转发的地址，即子系统的IP地址。
- connectport：要转发到的端口。

#### 参考资料

- [如何在局域网的其他主机上中访问本机的 WSL2](https://zhuanlan.zhihu.com/p/13899770349)