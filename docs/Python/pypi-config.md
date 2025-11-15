# PyPI配置镜像源

- Windows在用户目录创建配置文件`C:/Users/<用户名>/pip/pip.ini`
- Linux在家目录创建配置文件`~/.pip/pip.conf`

```
[global]
timeout = 6000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```

#### 参考资源

- [PyPI 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)