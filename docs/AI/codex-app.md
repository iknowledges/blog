# Codex App教程

## 配置网络代理

1. 新建如下文件：

- Linux/macOS: `~/.codex/.env`
- Windows: `%USERPROFILE%\.codex\.env`

2. 填写如下内容：

```
HTTP_PROXY="http://127.0.0.1:1080"
HTTPS_PROXY="http://127.0.0.1:1080"
```