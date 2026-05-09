# Antigravity使用Proxifier教程

1. 下载[Proxifier](https://www.proxifier.com/download/)并安装，注册可以参考[Proxifier-Keygen](https://github.com/y9nhjy/Proxifier-Keygen)。
2. 打开【Profile】->【Proxy Servers】->【Add】，输入如下配置：

- Address: 127.0.0.1
- Port: 7890
- Protocol: SOCKS Version 5

点击【OK】后会弹出是否设置默认代理，选择【No】

3. 打开【Profile】->【Proxification Rules】->【Add】，输入如下配置：

- Name: Antigravity (名称随意设置)
- Applications: Antigravity.exe; language_server_windows_x64.exe (这里的名称必须和任务管理器中的进程名称一致)
- Action: 选择上一步设置的代理服务

4. 最后打开Antigravity查看是否正常工作。

5. 另外也可以用开源软件[ProxyBridge](https://github.com/InterceptSuite/ProxyBridge)代替Proxifier。

#### 参考资料

- [使用Proxifier解决烦人的Tun](https://zhuanlan.zhihu.com/p/1998700433091351756)
- [Antigravity 非 TUN 代理方案ProxyBridge (Proxifier平替方案)](https://linux.do/t/topic/1688640)