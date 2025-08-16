# SSh密钥远程连接

## 服务端

1. 生成本地密钥对

```bash
ssh-keygen -t rsa
```

注：如果是从别处拷贝过来的密钥，需要修改权限

```bash
cd ~/.ssh
chmod 600 id_rsa
chmod 644 id_rsa.pub
```

2. 远程服务器授权

```bash
cd ~/.ssh
vim authorized_keys
```

将上一步生成的id_rsa.pub文件内容添加到authorized_keys的末尾。

3. 修改ssh配置文件

```bash
vim /etc/ssh/sshd_config
```

修改如下：

```
PermitRootLogin yes
PubkeyAuthentication yes
AuthorizedKeysFile /root/.ssh/authorized_keys
PasswordAuthentication yes
```

4. 最后重启服务

```
systemctl restart sshd
```

## 客户端

### Tabby

打开Tabby客户端，依次选择：【Profiles & connections】->【ADVANCED】->【SSH】->【Add a private key】，然后选择id_rsa文件。

### VSCode

打开Remote - SSH工具，打开配置文件config，修改内容如下：

```
Host remotehost
  User root
  HostName 192.168.3.10
  IdentityFile C:\Users\abc\.ssh\id_rsa
```

- Host: 名字随便填
- User: 登录用户名
- HostName: 登录IP地址
- IdentityFile: 本地私钥路径

#### 参考资源

- [用Tabby服务器远程SSH登录](https://blog.csdn.net/xeqja/article/details/128815491)
- [Remote Development using SSH](https://code.visualstudio.com/docs/remote/ssh#_remember-hosts-and-advanced-settings)
