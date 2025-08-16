# JVM处理OOM问题

## 获取Dump文件的两种方式

1. JVM启动时增加两个参数:

```
# 出现 OOME 时生成堆 dump
-XX:+HeapDumpOnOutOfMemoryError
# 生成堆文件地址
-XX:HeapDumpPath=/home/liuke/jvmlogs/
```

2. 发现程序异常前通过执行指令，直接生成当前JVM的dmp文件，6214是指JVM的进程号

```
jmap -dump:format=b,file=/home/admin/logs/heap.hprof 6214
```

## 其他常用命令

```
# 查看整个JVM内存状态
jmap -heap [pid]
# 查看JVM堆中对象详细占用情况
jmap -histo [pid]
```

#### 参考资料

- [JVM调优 dump文件怎么生成和分析](https://www.cnblogs.com/myseries/p/10827195.html)
