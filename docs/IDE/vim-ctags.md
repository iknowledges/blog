# Vim使用ctags查看源代码

1. 安装ctags

```
sudo apt install ctags
```

2. cd到源代码目录并生成代码索引

```
# 生成索引文件
ctags -R
# 查看索引文件
ls -l tags
```

3. vim打开文件

```
# 打开文件
vim net/ipv4/af_inet.c
# 根据函数名打开所在文件
vim -t tcp_v4_rcv
```

4. 常用操作

```
ctrl+] 跳转到函数或变量定义
g+ctrl+] 跳转到函数或变量定义
ctrl+o 返回
```
