# Tailescale安装教程

## 自建中转服务器

1. 安装依赖包

```
apt update && apt upgrade
apt install -y wget git openssl curl
```

2. 安装go编译环境

```
# 下载安装包
wget https://golang.google.cn/dl/go1.21.6.linux-amd64.tar.gz
# 解压到系统目录
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.21.6.linux-amd64.tar.gz
# 添加环境变量
echo "export PATH=$PATH:/usr/local/go/bin" >> /etc/profile
source /etc/profile
# 测试是否安装成功
go version

# 拉取编译derper中转服务器
go install tailscale.com/cmd/derper@main
```

- 配置国内源（可选）

```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

3. 修改源码跳过域名设置

```
cd /root/go/pkg/mod/tailscale.com@v1.1.1-0.20240131235917-b4b2ec780159/cmd/derper/
vim cert.go
```

修改如下源码：

```go
func (m *manualCertManager) getCertificate(hi *tls.ClientHelloInfo) (*tls.Certificate, error) {
    // 注释下面三行
    //if hi.ServerName != m.hostname {
    //    return nil, fmt.Errorf("cert mismatch with hostname: %q", hi.ServerName)
    //}

    // Return a shallow copy of the cert so the caller can append to its
    // Certificate field.
    certCopy := new(tls.Certificate)
    *certCopy = *m.cert
    certCopy.Certificate = certCopy.Certificate[:len(certCopy.Certificate):len(certCopy.Certificate)]
    return certCopy, nil
}
```

在该目录下重新进行编译：

```
go build -o $HOME/go/bin/derper
```

4. 随便取一个域名，如myderp.com，使用如下命令生成证书：

```
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 -nodes -keyout /home/derp/myderp.com.key -out /home/derp/myderp.com.crt -subj "/CN=myderp.com" -addext "subjectAltName=DNS:myderp.com"
```

5. 创建一个derp系统服务，防火墙打开33445和33446端口

```
cat > /etc/systemd/system/derp.service <<EOF
[粘贴文件内容]
> EOF
```

derp.service的内容如下：

```
[Unit]
Description=TS Derper
After=network.target
Wants=network.target
[Service]
User=root
Restart=always
ExecStart=$HOME/go/bin/derper -hostname myderp.com -a :33445 -http-port 33446 -certmode manual -certdir /home/derp
RestartPreventExitStatus=1
[Install]
WantedBy=multi-user.target
```

启动服务，然后用浏览器访问https://ip:33445：

```
# 设置自启动
systemctl enable derp
systemctl start derp
```

6. 通过客户端进入官网，打开Access controls标签页，添加derpMap部分的配置：

```
{
  // ... other parts of ACL
  "derpMap": {
    "OmitDefaultRegions": true,
    "Regions": {
      "900": {
        "RegionID": 900,
        "RegionCode": "myderp",
        "RegionName": "myderp server",
        "Nodes": [
          {
            "Name": "1",
            "RegionID": 900,
            "DERPPort": 33445,
            "HostName": "your-hostname.com",
            // "IPv4": "192.168.23.456",
            // "IPv6": "fd7a:115c:a1e0::83a1:9801",
            "InsecureForTests": true,
          }
        ]
      }
    }
  },
}
```

- OmitDefaultRegions: 禁用其他官方的中转服务器
- DERPPort: 中转服务器的端口
- IPV4和IPv6: 中转服务器的IP地址

7. 客户端操作

```
# 查看是否连接了中转服务器
tailscale netcheck
# 查看网络状态
tailscale status
# 查看网络延时
tailscale ping 100.106.168.51
# 客户端下线
tailscale down
# 客户端上线
tailscale up
```

### 云服务配置IPv6（可选）

如果云服务器没有公网IPv6，可以访问这个网址：https://tunnelbroker.net/

1. 注册后依次选择【Create Regular Tunnul】->【Create Tunnel】,创建成功后选择【Example Configurations】，再选择ubuntu，生成如下配置：

```
auto he-ipv6
iface he-ipv6 inet6 v4tunnel
        address 2001:470:35:8ae::2
        netmask 64
        endpoint 216.218.221.42
        local 192.168.23.456
        ttl 255
        gateway 2001:470:35:8ae::1
```

- local: 修改为云服务器私有IP，可通过`ip addr`命令查看私有IP
- 注意：云服务器的防火墙需要打开ICMP协议

2. 将配置添加到云服务器的配置文件`/etc/network/interfaces`：

```
vim /etc/network/interfaces
reboot
```

### 防止其他人使用自建中转服务器（推荐）

1. 云服务器安装tailscale

```
curl -fsSL https://tailscale.com/install.sh | sh
# 运行下面命令启动，然后访问显示的链接进行授权
tailscale up
```

2. 修改配置

```
vim /etc/systemd/system/derp.service
```

在启动命令后添加参数--verify-clients，如：

```
......
ExecStart=/root/go/bin/derper -hostname myderp.com -a :33445 -http-port 33446 -certmode manual -certdir /home/derp --verify-clients
......
```

3. 最后重启服务

```
# 重新加载服务
systemctl daemon-reload
# 重启
systemctl restart derp
```

### 配置DREP区域

查看默认的区域和ID

```
sudo apt install jq
curl --silent https://controlplane.tailscale.com/derpmap/default | jq -r '.Regions[] | "\(.RegionID) \(.RegionName)"'
```

返回结果如下：

```
1 New York City
10 Seattle
11 São Paulo
12 Chicago
13 Denver
14 Amsterdam
15 Johannesburg
16 Miami
17 Los Angeles
18 Paris
19 Madrid
2 San Francisco
20 Hong Kong
21 Toronto
22 Warsaw
23 Dubai
24 Honolulu
25 Nairobi
3 Singapore
4 Frankfurt
5 Sydney
6 Bangalore
7 Tokyo
8 London
9 Dallas
```

将指定区域设置为null，客户端就不再连接这个区域，下面3代表新加坡，注释掉表示可以连接：

```
{
  // ... other parts of ACL
  "derpMap": {
    "Regions": {
      "1": null,
      "2": null,
      //"3": null,
      "4": null,
      "5": null,
      "6": null,
      "7": null,
      "8": null,
      "9": null,
      "10": null,
      "11": null,
      "12": null,
      "13": null,
      "14": null,
      "15": null,
      "16": null,
      "17": null,
      "18": null,
      "19": null,
      "20": null,
      "21": null,
      "22": null,
      "23": null,
      "24": null,
      "25": null,
    }
  }
}
```

### Windows登录前启动

1. win+r输入taskschd.msc，启动任务计划程序。

2. 点击【创建基本任务】，输入：

- 名称：tailscale
- 描述：tailscale登录前启动

3. 下一步【触发器】，选择【计算机启动时】。

4. 下一步【操作】，选择【启动程序】。

5. 下一步【启动程序】，【程序或脚本】输入tailscale程序路径，如"D:\Program Files\Tailscale\tailscale-ipn.exe"，然后点击【完成】。

6. 回到【任务计划程序库】，找到刚刚创建的tailscale，右键选择【属性】。

7. 【常规】->【安全选项】，选择【不管用户是否登录都要运行】和【使用最高权限】。

8. 点击【更改用户或组】，输入你的用户名，点击【检查名称】，然后依次点击【确定】。

#### 参考资源

- [Tailscale玩法](https://www.bilibili.com/video/BV1Wh411A73b)
- [Custom DERP Servers](https://tailscale.com/kb/1118/custom-derp-servers)
- [Tailscale开机启动](https://www.anubis.cafe/fd271dcd)
