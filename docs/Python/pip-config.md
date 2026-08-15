# pip配置

## 设置镜像源

- Windows在用户目录创建配置文件`C:/Users/<用户名>/pip/pip.ini`
- Linux在家目录创建配置文件`~/.pip/pip.conf`

```
[global]
timeout = 6000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```

## 问题解决

- 用pip安装依赖时出现如下报错：

```
error: externally-managed-environment
```

打开配置文件：

```
sudo vim /etc/pip.conf
```

添加如下内容：

```
[global]
break-system-packages = true
```

#### 参考资源

- [PyPI 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)
- [How to fix error: externally-managed-environment in Python 3.12 the Wrong Way](https://www.youtube.com/watch?v=g2TDfWDgwkE)
