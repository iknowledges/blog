# VSCode配置Python环境

## 配置python模块查找路径

- 方法一：配置settings.json:

```
{
    // linux
    "terminal.integrated.env.linux": {
        "PYTHONPATH": "${workspaceFolder}"
    }
    // widnows
    "terminal.integrated.env.windows": {
        "PYTHONPATH": "${workspaceFolder}"
    }
}
```

可以通过运行以下代码查看查找模块的路径：

```python
import sys

print(sys.path)
```

- 方法二：配置launch.json:

```
{
    "version": "0.2.0",
    "configurations": [
        {
            ......
            "env": {"PYTHONPATH": "${workspaceRoot}"}
        }
    ]
}
```

## 配置python格式化

格式化快捷键：alt+shift+f

> **autopep8**

1. 安装autopep8插件
2. 配置settings.json

```
{
    "[python]": {
        "editor.defaultFormatter": "ms-python.autopep8",
        "editor.formatOnSave": true
    },
    "autopep8.args": [
        "--aggressive",
        "--max-line-length=120",
        "--ignore=E402",
        "--exclude=venv",
        "--ignore-local-config"
    ]
}
```

- aggressive: 这个选项表示在格式化代码时使用更激进的策略。它会应用更多的代码转换规则，以尽可能地改进代码的格式和风格。
- max-line-length=120: 这个选项指定每行代码的最长字符数限制。
- ignore=E402: 这个选项告诉autopep8忽略指定的错误代码。在这种情况下，E402表示 “module level import not at top of file”，即模块级别的导入不在文件顶部。这个选项可以用来避免autopep8对这类错误的修复。
- exclude=venv: 这个选项指定要排除的目录或文件，“venv”是要排除的目录名。autopep8将不会对这个目录下的文件进行格式化。
- ignore-local-config: 这个选项告诉autopep8忽略本地配置文件。autopep8通常会查找并应用项目目录中的配置文件，如 .editorconfig 或 pyproject.toml。

> **black**

1. 安装Black Formatter插件
2. 配置settings.json

```
{
    "[python]": {
        "editor.defaultFormatter": "ms-python.black-formatter",
        "editor.formatOnSave": true
    },
    "black-formatter.args": [
        "--skip-string-normalization",
        "--line-length",
        "120"
    ]
}
```

- skip-string-normalization: black默认会将单引号格式化为双引号，这个配置可以跳过这个操作。
- line-length: 指定每行代码最长字符数限制。

## 代码提示

1. 安装Pylance
2. 配置settings.json

```
{
    "python.languageServer": "Pylance"
}
```

## 自定义代码配色

```
{
    "editor.tokenColorCustomizations": {
        "comments": "#55aa7f",
        "keywords": "#ff55ff",
        "variables": "#a792e2",
        "strings": "#00ff7f",
        "functions": "#ffff00",
        "numbers": "#00eeff",
        "types": "#55bbff",
    }
}
```

- comments: 注释
- keywords: 关键字
- variables: 变量名
- strings: 字符串
- functions: 函数名
- numbers: 数字
- types: 类名


#### 参考资料

- [Use of the PYTHONPATH variable](https://code.visualstudio.com/docs/python/environments#_use-of-the-pythonpath-variable)
- [解决vscode找不到Python自定义模块](https://blog.csdn.net/sxww_zyt/article/details/129487582)
