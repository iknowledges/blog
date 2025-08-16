# Doxygen和Graphviz生成UML图

1. 安装软件：

```
sudo apt install graphviz
sudo apt install doxygen
```

2. 进入源代码目录，生成配置文件：

```
doxygen -g Doxygen.config
```

3. 修改Doxygen.config配置：

```
EXTRACT_ALL            = YES
HAVE_DOT               = YES
UML_LOOK               = YES
RECURSIVE              = YES
```

4. 生成文档：

```
doxygen Doxygen.config
```
