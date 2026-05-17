# Windows embeddable package安装教程

1. 下载[python-3.14.4-embed-amd64.zip](https://www.python.org/downloads/windows/)并解压。
2. 修改`python314._pth`文件：

```
python314.zip
.

# Uncomment to run site.main() automatically
import site
```

3. 安装pip

```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python.exe get-pip.py
python.exe -m pip --version
```

4. 将如下路径加入系统环境变量：

```
\path\to\python-3.14.4-embed-amd64
\path\to\python-3.14.4-embed-amd64\Scripts
```

5. 由于Embeddable Package不包含[Standard Library Projects](https://packaging.python.org/en/latest/key_projects/#standard-library-projects)，如果需要使用venv模块只能手动安装virtualenv

```
pip install vertualenv
```

#### 参考资料

- [Installing Packages](https://packaging.python.org/en/latest/tutorials/installing-packages/#ensure-pip-setuptools-and-wheel-are-up-to-date)
- 
- [Python Embeddable Package 完全指南](https://www.jermey.cn/2026/04/08/python-embeddable-package-guide.html)
