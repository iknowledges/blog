# ClickHouse安装教程

1. 安装

```
curl https://clickhouse.com/ | sh
# 注意安装过程中会提示设置用户密码
sudo ./clickhouse install
```

2. 启动服务

```
sudo clickhouse start
sudo clickhouse stop
sudo clickhouse status
```

3. 客户端连接

```
clickhouse-client --password
```