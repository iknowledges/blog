# Ubuntu编译Redis

1. 官网下载[redis-6.2.14.tar.gz](https://redis.io/downloads/)并解压

```
tar -zxvf redis-6.2.14.tar.gz
```

2. 安装OpenSSL（不使用BUILD_TLS=yes可不安装）

```
sudo apt install libssl-dev
```

3. 编译安装

```
cd redis-6.2.14/
make BUILD_TLS=yes
sudo make PREFIX=$MY_INSTALL_DIR/redis install
```

4. 启动redis

```
cd $MY_INSTALL_DIR/redis/bin
# 启动服务端
./redis-server
# 启动客户端
./redis-cli
```

#### 错误处理

- 报错信息如下：

```
zmalloc.h:50:10: fatal error: jemalloc/jemalloc.h: No such file or directory
   50 | #include <jemalloc/jemalloc.h>
```

解决方法：使用下面命令清理上次残留文件，再重新编译

```
make distclean
```

#### Redis的Windows版本下载地址

下载[Redis-x64-5.0.14.1.zip](https://github.com/tporadowski/redis/releases)然后解压，执行下面命令启动Redis：

```
redis-server.exe redis.windows.conf
```

#### 参考资料

- [Install Redis from Source](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-from-source/)
- [redis](https://github.com/redis/redis)
