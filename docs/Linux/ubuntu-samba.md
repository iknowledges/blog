# Ubuntu20.04安装samba

1. 安装samba服务

```
sudo apt install samba
```

2. 创建共享文件目录

```
mkdir /home/sheeran/Desktop/sambashare
```

3. 修改samba配置

```
sudo vim /etc/samba/smb.conf
```

在最后面填入下面内容：

```
[share]
   comment = Samba on Ubuntu
   path = /home/sheeran/Desktop/sambashare
   browseable = yes
   read only = no
```

4. 重启服务

```
sudo service smbd restart
```

5. 配置防火墙规则，设置用户密码

```
sudo ufw allow samba
sudo smbpasswd -a sheeran
```

6. 最后在Windows的资源管理器访问`\\192.168.0.39\share`
