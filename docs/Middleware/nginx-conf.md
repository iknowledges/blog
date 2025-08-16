# nginx配置问题

## Nginx 的相关配置

```
# worker进程的数量
worker_processes  1;

# 事件区块开始
events {
    # 每个worker进程支持的最大连接数
    worker_connections  1024;
}
# 事件区块结束

# HTTP区块开始
http {
    # Nginx支持的媒体类型库文件
    include       mime.types;
    # 默认的媒体类型
    default_type  application/octet-stream;
    # 开启高效传输模式
    sendfile        on;
    # 连接超时
    keepalive_timeout  65;
    # 第一个Server区块开始，表示一个独立的虚拟主机站点
    server {
        # 提供服务的端口，默认80
        listen       80;
        # 提供服务的域名主机名       
        server_name  localhost;
        # 第一个location区块开始
        location / {
            # 站点的根目录，相当于Nginx的安装目录
            root   html;
            # 默认的首页文件，多个用空格分开
            index  index.html index.htm;
        }
        # 第一个location区块结束

        # 出现对应的http状态码时，使用50x.html回应客户端
        error_page   500 502 503 504  /50x.html;
        # location区块开始，访问50x.html
        location = /50x.html {
            # 指定对应的站点目录为html
            root   html;
        }
    }
}
```

## 正向代理

```
server {
    listen       80;
    server_name  localhost;

    location / {
        # 将请求转发到指定的服务器
        proxy_pass http://example.com;
        # 设置请求头中的Host字段
        proxy_set_header Host $host;
        # 设置请求头中的真实IP地址
        proxy_set_header X-Real-IP $remote_addr;
        # 设置请求头中的转发地址
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        # 设置请求头中的协议类型
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

以上配置将客户端的请求转发到http://example.com，并且在转发过程中设置了一些请求头信息，以便服务器能够正确处理请求。

## 反向代理

```
location / {
    proxy_pass http://backend_server;
}
```

其中backend_server是真实服务器的地址。这样，当客户端发送请求时，Nginx会将请求转发到backend_server上，并将响应返回给客户端。

## root和alias的区别：

- root的处理结果是：root路径拼接location路径，root指定的目录下必须有location的文件夹。
- alias的处理结果是：使用alias路径替换location路径，alias指定的目录必须以“/”结尾。

```
# http://localhost/picture/访问的是根目录下的demo/picture文件夹
location /picture/ {
    root ./demo/;
    autoindex on;
}

# http://localhost/picture/访问的是上级目录下的demo文件夹
location /picture/ {
    alias ../demo/;
    autoindex on;
}
```

## 负载均衡配置

```
upstream lb {
    server 127.0.0.1:8080 weight=1;
    server 127.0.0.1:8081 weight=1;
}

location / {
    proxy_pass http://lb;
}
```

## 常见问题

- Permission denied

```
# 查看worker进程的用户
ps -ef | grep nginx
```

修改/etc/nginx/nginx.conf中的user：

```
user  root;
```

然后重新加载配置：

```
nginx -s reload
```

- 控制台报错：The Cross-Origin-Opener-Policy header has been ignored, because the URL's origin was untrustworthy. It was defined either in the final response or a redirect. Please deliver the response using the HTTPS protocol. You can also use the 'localhost' origin instead.

```
location / {
    # 设置响应头
    add_header 'Cross-Origin-Opener-Policy' 'same-origin';
    add_header 'Cross-Origin-Embedder-Policy' 'require-corp';
}
```

- 报错：upstream sent too big header while reading response header from upstream

```
http {
    proxy_buffer_size  64k;
    proxy_buffers   32 64k;
    proxy_busy_buffers_size 128k;
}
```

#### 参考资源

- [一文教你学会使用Nginx](https://mp.weixin.qq.com/s/OIn5SgbDtFNEnbeNITWsKw)
- [Nginx搭建Google镜像站](https://blog.stdio.io/689)
- [Nginx反向代理使用的一些坑](https://www.yeeach.com/post/1497)
- [nginx报upstream sent too big header while reading response header from upstream](https://blog.csdn.net/yyj108317/article/details/109484923)
