# qmd教程

## 安装

```
npm install -g @tobilu/qmd
```

## 使用

1. 创建本地文档索引

```
qmd collection add /path/to/your/docs --name my-docs
# 生成向量embeddings，需要下载模型
qmd embed
# 查看当前状态
qmd status
```

2. 启动mcp服务

```
qmd mcp --http --host 0.0.0.0 --port 8181
```

Agent连接设置：

```json
{
  "mcpServers": {
    "qmd": {
      "type": "streamable-http",
      "url": "http://localhost:8181/mcp"
    }
  }
}
```

3. 更新文档

```
qmd update
qmd embed
```

#### 参考资料

- [qmd](https://github.com/tobi/qmd)
