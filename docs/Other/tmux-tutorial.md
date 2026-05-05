# tmux教程

## 安装

```
sudo apt install tmux
```

## 基本命令

```
# 创建会话
tmux new -s basic
# 退场当前会话
exit
# 列出会话
tmux ls
# 打开已存在的会话
tmux attach -t basic
# 关闭会话
tmux kill-session -t basic
```

## 快捷键

```
# 让当前会话在后台运行
Ctrl+b+d
```