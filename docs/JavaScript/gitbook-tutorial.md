# GitBook教程

## 安装命令：

```
npm install -g gitbook-cli
# 查看是否安装成功
gitbook -V
```

## 常用命令：

```
mkdir gitbook-demo
# 初始化项目
gitbook init
# 启动项目
gitbook serve
# 构建静态网页，默认输出到_book/目录
gitbook build
```

- 使用python打开gitbook：

```
cd /path/to/_book
python -m http.server
```