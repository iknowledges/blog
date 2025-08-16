# Windows常用命令

1. 根据端口号来查询进程ID

```
netstat -ano | findstr "端口号"
```

2. 根据进程ID来查询进程名

```
tasklist | findstr 进程ID
```

3. 定时关机

```
# 3600秒，也就是1小时
shutdown -s -t 3600
# 取消关机
shutdown -a
```