# letsencry教程

## 申请单个域名证书

1. 安装系统依赖

```bash
# Ubuntu
sudo apt install python3 python3-venv libaugeas0
# CentOS
dnf install python3 augeas-libs
```

2. 卸载老版本

```bash
# Ubuntu
sudo apt-get remove certbot
# CentOS
sudo dnf remove certbot
sudo yum remove certbot
```

3. 创建python虚拟环境

```bash
sudo python3 -m venv /opt/certbot/
sudo /opt/certbot/bin/pip install --upgrade pip
```

4. 安装Cerbot

```bash
source /opt/certbot/bin/activate
pip install certbot==2.11.1
# 如果安装pyOpenSSL出现与acme版本冲突就要降版本
pip install acme==2.11.1
# 解决报错：ValueError: Invalid version. The only valid version for X509Req is 0.
pip install pyOpenSSL==23.1.1
# 验证是否成功
certbot --version
```

5. 运行Cerbot

```bash
certbot certonly --standalone -d mydomain.top
```

参考如下选择：

```
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.3-September-21-2022.pdf. You must
agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Account registered.
```

认证成功后会生成如下文件：

```
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/mydomain.top/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/mydomain.top/privkey.pem
```

## 申请泛域名证书

1. CloadFlare创建令牌：

依次选择：【网站】->【获取您的API令牌】->【创建令牌】

使用编辑区域DNS模板，权限部分必须选择【区域:DNS:编辑】，其他默认，然后记录下随机生成的令牌。

2. 创建~/.secrets/certbot/cloudflare.ini，写入刚刚记录的令牌：

```
# Cloudflare API token used by Certbot
dns_cloudflare_api_token = 0123456789abcdef0123456789abcdef01234567
```

3. 参考第一部分安装好certbot，另外还要安装DNS插件：

```bash
pip install certbot-dns-cloudflare
```

4. 生成泛域名证书

```bash
certbot certonly --dns-cloudflare --dns-cloudflare-credentials ~/.secrets/certbot/cloudflare.ini -d mydomain.top -d *.mydomain.top
```

## 证书续期

```
# 查看证书信息
certbot certificates
# --force-renewal强制更新证书，无视30天之内限制
certbot renew --cert-name mydomain.top --force-renewal
```

手动续期需要注意两点：

1. 80端口没有被占用，因为更新证书需要用到80端口。
2. 在到期前30天之内才能续期，否则certbot会判断没有必要进行续期。

#### 参考资源

- [certbot instructions](https://certbot.eff.org/instructions?ws=webproduct&os=pip)
- [使用let's encrypt申请免费的SSL证书](https://developer.aliyun.com/article/1246358)
- [Welcome to certbot-dns-cloudflare’s documentation](https://certbot-dns-cloudflare.readthedocs.io/en/stable/)
- [Wildcard SSL Certificates with Certbot + Cloudflare](https://labzilla.io/blog/cloudflare-certbot)
- [Certbot免费证书的安装·使用·自动续期](https://blog.csdn.net/qq_42760638/article/details/117962788)
