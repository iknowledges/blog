# Claude Code安装教程

1. 参考[Claude Code Docs](https://code.claude.com/docs/zh-CN/setup)进行安装：

- macOS, Linux, WSL中安装

```
curl -fsSL https://claude.ai/install.sh | bash
```

- NPM 安装（已弃用）

```
npm install -g @anthropic-ai/claude-code
```

2. 验证安装

```
claude --version
```

2. 参考[环境变量](https://code.claude.com/docs/zh-CN/settings)配置API，修改~/.claude/settings.json如下：

```json
{
    "env": {
        "ANTHROPIC_AUTH_TOKEN": "你的APY KEY",
        "ANTHROPIC_BASE_URL": "https://api.moonshot.cn/anthropic/"
    }
}
```

如果配置没生效，可直接设置环境变量：

```
export ANTHROPIC_AUTH_TOKEN="你的APY KEY"
export ANTHROPIC_BASE_URL="https://api.moonshot.cn/anthropic/"
```

3. 启动Claude Code

```
claude
```

4. （可选）配置网络代理，在项目根目录创建.claude/settings.json，内容如下：

```json
{
    "env": {
        "HTTP_PROXY": "http://127.0.0.1:1080",
        "HTTPS_PROXY": "http://127.0.0.1:1080"
    }
}
```