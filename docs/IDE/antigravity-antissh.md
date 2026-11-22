# Antigravity使用antissh教程

1. 在Ubuntu中安装go：

```
sudo apt install golang-go
```

2. 安装graftcp：

```
git clone https://github.com/hmgle/graftcp.git ~/.graftcp-antigravity/graftcp
make -C ~/.graftcp-antigravity/graftcp
```

3. 下载antissh脚本并执行：

```
curl -fLO https://raw.githubusercontent.com/ccpopy/antissh/main/antissh.sh
bash antissh.sh check socks5://127.0.0.1:10808
# 关闭对应的 IDE 远程窗口后，再安装 wrapper
bash antissh.sh install socks5://127.0.0.1:10808
bash antissh.sh status
```

安装成功后显示如下内容：

```
目标：/home/ubuntu/.gemini/bin/agy
状态：native
原始 agy 版本：1.5.14
代理连通性校验通过（HTTP 404）。
已安装代理 wrapper：/home/ubuntu/.gemini/bin/agy
原始二进制备份：/home/ubuntu/.gemini/bin/agy.antiagy-original
请重新连接 IDE 远程窗口；更新 agy 后可重运行本脚本。
```

#### 参考资料

- [antissh](https://github.com/ccpopy/antissh)
