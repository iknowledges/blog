# iSH教程

## iSH换源

编辑配置文件

```
vi /etc/apk/repositories
```

增加如下内容：

```
http://apk.ish.app/v3.14/main
http://apk.ish.app/v3.14/community
```

然后更新源

```
apk update
```

## 安装软件

```
# 搜索软件
apk search openssh
# 安装openssh
apk add openssh
```
