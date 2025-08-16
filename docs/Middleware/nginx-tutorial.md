# 30分钟Nginx入门教程

## Nginx的安装

方式一：包管理器

```
# ubuntu
sudo apt update
sudo apt install nginx
# macos
brew install nginx
```

方式二：编译安装

```
./configure --sbin-path=/usr/local/nginx/nginx --conf-path=/usr/local/nginx/nginx.conf --pid-path=/usr/local/nginx/nginx.pid --with-http_ssl_module --with-pcre=../pcre2-10.39 --with-zlib=../zlib-1.2.11
make
make install 
```

## 服务启停

```
# 启动服务
nginx
# 查看nginx进程
ps -ef | grep nginx
# 查看端口占用情况
lsof -i:80
# 优雅停止
nginx -s quit
# 立即停止
nginx -s stop
# 重载配置文件
nginx -s reload
# 重新打开日志文件
nginx -s reopen
# 检查配置文件是否有错误
nginx -t
```

## 静态站点部署

执行下面命令查看nginx的相关信息：
- --conf-path表示配置文件路径
- --prefix表示安装目录

```
nginx -V
```

配置文件内容如下：

```
server {
    ......

    location / {
        root   html;
        index  index.html index.htm;
    }

    ......
}
```

- root代表根目录对应的文件夹，是相对于nginx安装目录的相对路径
- index是根目录下的主页文件

使用Hexo生成一个博客网站：

```
npm install hexo-cli -g
# 初始化hexo项目
hexo init blog-demo
cd blog-demo
# 把markdown文件转换成静态页面，放到public文件夹下
hexo g
# 启动本地服务器
hexo s
# 将静态页面复制到nginx静态文件目录
cp -rf ./public/* /opt/homebrew/var/www
# 另外有个一键部署命令
hexo d
```

## 配置文件

配置文件的结构主要由三个配置块组成：

1. 全局块

- worker_processes: 配置worker进程数量，一般和服务器CPU内核数量相同比较合适，可以设置为auto，这样Nginx就会根据内核数量来自动设置worker进程的数量。

2. events块：配置服务器和客户端之间的网络连接。

- worker_connections: 指定每个worker进程可以同时接收多少个网络连接。

3. http块：配置虚拟主机（server块）、反向代理和负载均衡等。

- include mime.types: 表示把mime.types这个文件包含进来，这个文件中定义了很多MIME的类型，这样Nginx就可以根据文件的后缀名来判断文件类型，再根据不同的文件类型来进行不同的处理，比如html被当作文本文件处理，jpg文件被当作二进制文件处理。
- include servers/*: 表示servers目录下所有的配置文件都包含进来，这样可以把每个虚拟主机的配置放到一个单独的文件里面。

## 反向代理和负载均衡

正向代理和反向代理：简单来说，正向代理是代理客户端，反向代理是代理服务端。

反向代理配置文件如下：

```
http {
    ......

    upstream backend {
        ip_hash;
        server 127.0.0.1:8000 weight=3;
        server 127.0.0.1:8001;
        server 127.0.0.1:8002;
    }

    server {
        listen       80;
        server_name  localhost;

        location /app {
            proxy_pass http://backend;
        }
    }

    ......
}
```

以上是将http://localhost/app代理到8000、8001和8002三个服务。

- weight: 服务以轮询方式进行代理，weight参数可以配置服务权重。
- ip_hash: 这个策略会根据客户端的ip地址来进行一个哈希，同一个客户端的请求就会被分配到同一个服务器上，这样就解决了一些session的问题。

## HTTPS配置

使用openssl生成证书：

- 生成私钥文件（private key）

```
openssl genrsa -out private.key 2048
```

- 根据私钥生成证书签名请求文件（Certificate Signing Request，简称CSR文件）

```
openssl req -new -key private.key -out cert.csr
```

- 使用私钥对证书申请进行签名从而生成证书文件（pem文件）

```
openssl x509 -req -in cert.csr -out cacert.pem -signkey private.key 
```

- 或者将以上三条命令合并成一条：

```
openssl req -x509 -newkey rsa:4096 -sha256 -nodes -keyout private.key -out cacert.pem -days 3650
```

命令运行完成会生成两个文件：一个私钥文件private.key，一个证书文件cacert.pem

修改配置文件如下：

```
server {
    listen      443 ssl;
    server_name localhost;
    # 证书文件名称
    ssl_certificate /opt/homebrew/etc/nginx/cacert.pem;
    # 证书私钥文件名称
    ssl_certificate_key /opt/homebrew/etc/nginx/private.key;
    # ssl验证配置
    ssl_session_timeout 5m;  # 缓存有效期
    # 安全链接可选的加密协议
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    # 配置加密套件/加密算法，写法遵循 openssl 标准。
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
    # 使用服务器端的首选算法
    ssl_prefer_server_ciphers on;

    ......
}
```

配置一个重定向，让http请求自动跳转到https：

```
server {
    listen      80
    server_name geekhour.net www.geekhour.net;
    return 301  https://$server_name$request_uri;
}
```


## 虚拟主机

虚拟主机是通过配置server块来实现的，我们可以在servers目录下新建vue.conf配置文件，并写入如下内容：

```
server {
    listen 5173;
    server_name localhost;
    location / {
        root /Users/yiny/vue-demo/dist;
        index index.html index.htm;
    }
}
```

#### 参考连接

- [【GeekHour】30分钟Nginx入门教程](https://www.bilibili.com/video/BV1mz4y1n7PQ)