# WSL2使用Clash Verge代理

1. 因为我的WSL2使用的是默认Nat网络模式，所有Clash Verge中要设置allow lan，允许局域网访问。

2. 设置window防火墙，放开7890端口：

```
netsh advfirewall firewall add rule name="Open Port 7890" dir=in action=allow protocol=TCP localport=7890
```

3. wsl中设置代理地址

```
# 查看window的IP地址
ip route show | grep -i default | awk '{ print $3}'
# 设置代理，host_ip替换为上面的IP
export https_proxy="http://${host_ip}:7890"
export http_proxy="http://${host_ip}:7890"
export all_proxy="socks5://${host_ip}:7890"
```

#### 参考资料

- [WSL2使用clash for windows代理](https://gist.github.com/libChan/3a804a46b532cc326a2ee55b27e8ac19?permalink_comment_id=5725982)