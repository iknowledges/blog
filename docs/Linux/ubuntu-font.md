# Ubuntu安装中文字体

1. 下载宋体文件：[Simsun Font](https://www.freefonts.io/simsun-font/)，并进行解压。

2. 执行下面命令安装字体：

```
cd /usr/share/fonts/truetype/
sudo mkdir win
mv ~/Downloads/SIMSUN.ttf ./win
# 生成字体缩放信息
sudo mkfontscale
# 创建字体目录文件
sudo mkfontdir
# 更新字体缓存，-f表示强制更新，-v表示显示详细信息
sudo fc-cache -fv
```

3. 检查已安装字体中是否有宋体：

```
# 查看所有字体
fc-list
# 查看中文字体
fc-list :lang=zh
```

#### 参考资源

- [Ubuntu 20.04 安装宋体](https://blog.csdn.net/LclLsh/article/details/132509872)
