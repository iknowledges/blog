# RustDesk自建服务器

## 服务端

1. 下载编译好的安装文件，解压后里面会有hbbr、hbbs和rustdesk-utils三个文件

```
wget https://github.com/rustdesk/rustdesk-server/releases/download/1.1.9/rustdesk-server-linux-amd64.zip
```

2. 在服务器上运行hbbs和hbbr

```
# port可以不填，默认端口是21117
nohup ./hbbs -r <hbbr运行所在主机的地址[:port]> >hbbs.log 2>&1 &
nohup ./hbbr >hbbr.log 2>&1 &
```

默认情况下，hbbs监听21115(tcp), 21116(tcp/udp), 21118(tcp)，hbbr监听21117(tcp), 21119(tcp)。务必在防火墙开启这几个端口， 请注意21116同时要开启TCP和UDP。

3. 查看key

```
cat id_ed25519.pub
```

## 客户端

下载地址：https://github.com/rustdesk/rustdesk/releases/latest

安装客户端后，打开【设置】->【网络】，ID服务器填写hbbs运行的服务器地址，key填写服务端的公钥。

#### 参考资源

- [自建服务器OSS](https://rustdesk.com/docs/zh-cn/self-host/rustdesk-server-oss/install/)