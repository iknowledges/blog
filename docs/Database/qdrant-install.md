# Qdrant安装教程

1. 下载[qdrant-web-ui](https://github.com/qdrant/qdrant-web-ui/releases)并解压，将解压后的dist文件重命名为static：

```
wget https://github.com/qdrant/qdrant-web-ui/releases/download/v0.2.15/dist-qdrant.zip
unzip dist-qdrant.zip
mv dist static
```

2. 下载[qdrant](https://github.com/qdrant/qdrant/releases)并解压，windows中可以下载`qdrant-x86_64-pc-windows-msvc.zip`：

```
wget https://github.com/qdrant/qdrant/releases/download/v1.18.2/qdrant-x86_64-unknown-linux-gnu.tar.gz
tar -zxvf qdrant-x86_64-unknown-linux-gnu.tar.gz
# 启动服务
./qdrant
```

3. 打开`http://localhost:6333/dashboard`进入管理页面。
