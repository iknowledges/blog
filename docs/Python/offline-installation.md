# 离线安装python环境

1. 首先导出当前环境：

```python
pip freeze > requirements.txt
```

2. 在当前目录新建文件夹myenv，并下载安装文件。

```python
pip download -d myenv -r requirements.txt
```

3. 复制requirements.txt和myenv到另一台电脑上，并进行安装：

```python
pip install --no-index --find-links=myenv -r requirements.txt
```
