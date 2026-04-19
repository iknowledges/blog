# Kafka安装教程

1. 先确保已经安装好了Jdk 17：

```
java --version
```

2. 官网下载[kafka_2.13-4.2.0.tgz](https://kafka.apache.org/community/downloads/)并解压。

3. 在源代码目录创建文件夹kraft-combined-logs，打开`config/server.properties`，将`log.dirs`的路径修改为刚刚创建的文件夹，注意路径不要有空格：

```
# log.dirs=/tmp/kraft-combined-logs
log.dirs=F:/Kafka/kafka_2.13-4.2.0/kraft-combined-logs
```

4. 生成UUID：

```
> .\bin\windows\kafka-storage.bat random-uuid
HkDj0tQqTMmx9vKSbGj-bQ
```

5. 格式化Log路径：

```
.\bin\windows\kafka-storage.bat format --standalone -t HkDj0tQqTMmx9vKSbGj-bQ -c .\config\server.properties
```

6. 启动服务

```
.\bin\windows\kafka-server-start.bat .\config\server.properties
```

## 测试服务

1. 新建topic：

```
.\bin\windows\kafka-topics.bat --create --topic test-topic --bootstrap-server localhost:9092
```

2. 创建生产者：

```
.\bin\windows\kafka-console-producer.bat --topic test-topic --bootstrap-server localhost:9092
```

3. 创建消费者：

```
.\bin\windows\kafka-console-consumer.bat --topic test-topic --from-beginning --bootstrap-server localhost:9092
```

#### 参考资料

- [Quickstart](https://kafka.apache.org/quickstart/)
- [Kafka 4.0 Windows Setup in 2025: KRaft Mode WITHOUT ZooKeeper!](https://youtu.be/tKv4APQnmfM)
- [Installing Apache Kafka on Windows 11 in 5 minutes](https://youtu.be/LX5LKBYHmyU)