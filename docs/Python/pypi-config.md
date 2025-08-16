# PyPI配置镜像源

Windows在用户目录新建配置文件C:/Users/<用户名>/pip/pip.ini，内容如下：

```
[global]
timeout = 6000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```

#### 参考资源

- [PyPI 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)