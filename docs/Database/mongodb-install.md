# MongoDB安装教程

1. 在官网下载[mongodb-windows-x86_64-8.2.7.zip](https://www.mongodb.com/try/download/community)

2. 解压zip包，并以管理员身份打开powershell，执行如下命令

```
cd path\to\mongodb-win32-x86_64-windows-8.2.7
# 创建数据存储目录
md data\db
# 启动mongodb
.\bin\mongod.exe --dbpath=.\data\db
```

#### 参考资料

- [安装 MongoDB Community Edition](https://www.mongodb.com/zh-cn/docs/manual/administration/install-community)