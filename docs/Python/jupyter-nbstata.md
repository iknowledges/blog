# nbstata安装

1. 安装jupyterlab和nbstata

```
pip install jupyterlab
pip install nbstata
```

注意Python和Stata版本的兼容性：

|Stata|Python|
|--|--|
|Stata 17|Python 3.10, 3.11, 3.12, and 3.13|
|Stata 18, 18.5, 19, and 19.5|Python 3.10, 3.11, 3.12, 3.13, and 3.14|

2. 安装Stata内核

```
python -m nbstata.install --sys-prefix --conf-file
```

使用--conf-file生成配置文件，文件会创建在如下路径：

- `[sys.prefix]/etc/nbstata.conf`: --sys-prefix代表当前系统环境的路径，如virtualenv 或 conda env；
- `~/.config/nbstata/nbstata.conf`: 如果不指定安装路径就保存到用户家目录。

3. 修改配置文件nbstata.conf如下：

```
[nbstata]
stata_dir = D:/Program Files/StataNow19
edition = mp
```

4. 启动JupyterLab

```
jupyter lab --notebook-dir "YOUR_PATH_HERE"
```

## 魔法指令

|Magic|Description|
|--|--|
|%browse|查看数据集
|%head|查看前 5 (或 N) 行|
|%tail|查看最后 5 (或 N) 行|
|%frbrowse|查看数据框|
|%frhead|查看前 5 (或 N) 个数据框|
|%frtail|查看最后 5 (或 N) 个数据框|
|%locals|列出暂元和它们的值|
|%delimit|打印当前分隔符|
|%help|显示 Stata 帮助|
|%set|设置单个配置选项|
|%%set|设置多个配置选项|
|%status|显示 Stata 或配置状态|
|%%echo|显示命令回显|
|%%noecho|不显示命令回显|
|%%quietly|静默所有单元输出，包括图表|

#### 参考资料

- [User Guide](https://hugetim.github.io/nbstata/user_guide.html)
- [Jupyter Notebook 与 Stata 交互：nbstata](https://www.lianxh.cn/news/e1b2ad40fc3d4.html)