# 安装OpenResty

1. 下载安装包：https://openresty.org/cn/download.html

```
wget https://openresty.org/download/openresty-1.21.4.3.tar.gz
```

2. 安装必需的依赖

```
yum install pcre-devel openssl-devel gcc curl make
```

3. 解压安装包

```
tar -xzvf openresty-1.21.4.3.tar.gz
```

4. 编译安装

```
cd openresty-1.21.4.3
./configure --with-http_sub_module --with-http_v2_module
make
make install
```

- --with-http_sub_module表示安装ngx_http_sub_module模块
- --with-http_v2_module表示安装ngx_http_v2_module模块

5. 修改环境变量

```
vi /etc/profile
```

添加如下内容：

```
export PATH=/usr/local/openresty/nginx/sbin:$PATH
```

6. 启动Nginx

```
cd /usr/local/openresty/nginx/
nginx -p `pwd`/ -c conf/nginx.conf
```

## 安装lua-zlib

```
git clone https://github.com/brimworks/lua-zlib.git
cd lua-zlib
cmake -DLUA_INCLUDE_DIR=/usr/local/openresty/luajit/include/luajit-2.1 -DLUA_LIBRARIES=/usr/local/openresty/luajit/lib -DUSE_LUAJIT=ON -DUSE_LUA=OFF
make
cp zlib.so /usr/local/openresty/lualib/zlib.so
```
