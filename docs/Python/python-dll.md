# Python加载dll和pyd文件

## 加载pyd的方法

- 方法一：设置环境变量`PYTHONPATH`
- 方法二：可以加入如下代码临时添加：

```python
import sys
sys.path.append('/path/to/pyd')
```

## 加载dll的方法

- python3.8以下的版本使用如下代码：

```python
import os
os.environ['path'] += '/path/to/dll;'
```

- python3.8及以上版本使用如下代码：

```python
import os
os.add_dll_directory("/path/to/dll")
```

## pybind11的DLL加载失败

问题描述：在开发电脑上使用pybind11和cmake编译好了pyd文件后，拷贝到另外一台电脑上使用时，import导包时报错：dll load failed。

解决方法：

1. 在开发电脑上打开Visual Studio的开发工具Developer Command Prompt for VS 2022，使用如下命令查看pyd的依赖：

```
dumpbin /dependents path/to/example.cp311-win_amd64.pyd
```

2. 然后用everything找到系统中的dll文件，将这些文件拷贝到另一台电脑pyd的同级目录下。

#### 参考资料

- [Python下dll、pyd搜索路径的问题](https://hadongzhu.com/archives/69)
- [pybind11 DLL 加载失败：如何排除故障和保障稳定运行？](https://www.bytezonex.com/archives/M-nUcvDz.html)
