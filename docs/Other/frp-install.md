# frp内网穿透教程

## 一、服务端Linux

1. 下载压缩包并解压

```
wget https://github.com/fatedier/frp/releases/download/v0.53.2/frp_0.53.2_linux_amd64.tar.gz
tar -zxvf frp_0.53.2_linux_amd64.tar.gz
# 删除不需要的客户端程序
rm -f frpc frpc.toml
```

2. 修改配置文件frpc.toml如下：

```
bindPort = 7000
```

3. 后台启动服务

```
nohup ./frps -c frps.toml >frps.log 2>&1 &
```

## 二、客户端Windows

1. 下载对应版本的安装包[frp_0.53.2_windows_amd64.zip](https://github.com/fatedier/frp/releases/download/v0.53.2/frp_0.53.2_windows_amd64.zip)

2. 将frp加入系统白名单：【病毒和威胁防护】->【管理设置】->【添加或删除排除项】，然后添加frp所在文件夹。

3. 修改配置文件如下：

```
serverAddr = "服务器IP地址"
serverPort = 7000

[[proxies]]
name = "desktop"
type = "tcp"
localIP = "127.0.0.1"
localPort = 3389
remotePort = 6000
```

- localPort: 本地服务端口
- remotePort: 用户访问服务端的端口，其流量被转发到对应的本地服务

4. 客户端启动命令：

```
.\frpc.exe -c frpc.toml
```

### 配置Windows开机启动

1. 下载[WinSW-x64](https://github.com/winsw/winsw/releases/download/v2.12.0/WinSW-x64.exe)，解压后重命名为WinSW.exe

2. 编写start.bat脚本：

```
@echo off
:home
frpc.exe -c frpc.toml
goto home
```

3. 编写配置WinSW.xml

```xml
<service>
  <id>Frpc</id>
  <name>Frpc Service</name>
  <description>A Service for Frpc</description>
  <executable>%BASE%\start.bat</executable>
  <logpath>%BASE%\logs</logpath>
  <log mode="roll-by-time">
    <pattern>yyyyMMdd</pattern>
  </log>
</service>
```

4. 执行命令

```
# 注册服务
WinSW.exe install
# 卸载服务
WinSW.exe uninstall
# 启动服务
WinSW.exe start
# 停止服务
WinSW.exe stop
```

## 客户端P2P

1. 配置要暴露到外网的机器上的frpc.toml文件

```
serverAddr = "服务器IP地址"
serverPort = 7000
# natHoleStunServer = "xxx"

[[proxies]]
name = "p2p_desktop"
type = "xtcp"
secretKey = "abcdefg"
localIP = "127.0.0.1"
localPort = 3389
```

- natHoleStunServer: 如果默认的STUN服务器不可用，可以配置一个新的STUN服务器
- secretKey: 只有共享密钥一致的用户才能访问该服务

2. 配置想要访问内网服务的机器上的frpc.toml文件

```
serverAddr = "服务器IP地址"
serverPort = 7000
# natHoleStunServer = "xxx"

[[visitors]]
name = "p2p_desktop_visitor"
type = "xtcp"
serverName = "p2p_desktop"
secretKey = "abcdefg"
bindAddr = "127.0.0.1"
bindPort = 6000
# keepTunnelOpen = false
```

- natHoleStunServer: 如果默认的STUN服务器不可用，可以配置一个新的STUN服务器
- serverName: 要访问的P2P代理的名称
- bindAddr和bindPort: 绑定本地服务和端口
- keepTunnelOpen: 如果需要自动保持隧道打开，将其设置为true

3. 远程访问127.0.0.1:6000即可访问内网机器

#### 参考资源

- [frp官方文档](https://gofrp.org/zh-cn/docs/)