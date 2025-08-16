# Ubuntu解压zip文件

1. 安装unzip

```bash
sudo apt-get install unzip
```

2. 命令

```bash
# 解压到当前目录
unzip test.zip
# 解压到指定文件夹
unzip -d temp test.zip
# 查看压缩包内容不解压
unzip -l test.zip
# 压缩文件和目录
zip -r test.zip demo.txt public/
# 压缩文件
zip test.zip demo.txt
```
