# VScode编写Snippets教程

### 打开Snippets配置文件

1. 使用`Ctrl+Shift+p`打开命令面板。
2. 选择Snippets: Configure Snippets，可选择全局配置、项目配置或指定编程语言配置。

### Snippets基本语法

```json
"Print to console": {
    "scope": "javascript,typescript",
    "prefix": "ps",
    "body": [
        "console.log('$1');",
        "$2"
    ],
    "description": "Log output to console"
}
```

- Print to console: 是Snippets的名称。
- scope: 是编程语言的作用域。
- prefix: 是触发该Snippets的命令，输入这个prefix后按Tab键就可以触发。
- body: 是具体补全的内容，$1和$2是光标的占位符，$1是第一次按Tab键补全后光标的位置，$2是再次按Tab键后光标的位置，$0是结束补全后光标的位置。
- description: 是描述内容。

#### 占位符默认值

语法：${1:default}

```json
"Practice Snippets": {
    "prefix": "ps",
    "body": "console.log('${1:JavaScript}');"
}
```

#### 占位符选项

语法：${1|option1,option2,option3|}

```json
"Practice Snippets": {
    "prefix": "ps",
    "body": "console.log('${1|JavaScript,CSS,HTML|}');"
}
```

#### 变量格式化

语法：${<变量名>/<正则匹配>/<替换内容>/<选项>}，选项可以是g（全局匹配）、i（忽略大小写），也可以不传。下面例子是将文件名中的`.`替换成`_`。

```json
"Practice Snippets": {
    "prefix": "ps",
    "body": "${TM_FILENAME/[\\.]/_/}"
}
```

### 为Snippets设置快捷键

1. 使用`Ctrl+Shift+p`打开命令面板，选择Preferences: Open Keyboard Shortcuts (JSON)。
2. 在配置文件中添加如下代码：

- 方法一：在args中编写Snippets

```json
{
    "key": "ctrl+k 1",
    "command": "editor.action.insertSnippet",
    "when": "editorTextFocus",
    "args": {
        "snippet": "console.log($1)$0"
    }
}
```

- 方法二：指定现有的Snippets

```json
{
    "key": "ctrl+k 1",
    "command": "editor.action.insertSnippet",
    "when": "editorTextFocus",
    "args": {
        "langId": "javascript",
        "name": "Print to console"
    }
}
```

- 编写一个去除剪切板换行符的Snippets快捷键

```json
{
    "key": "ctrl+shift+v",
    "command": "editor.action.insertSnippet",
    "when": "editorTextFocus",
    "args": {
        "snippet": "${CLIPBOARD/\r?\n+/ /g}"
    }
}
```

#### 参考资料

- [掌握VSCode Snippets，解锁极速编码体验](https://zhuanlan.zhihu.com/p/25568298206)
- [Snippets in Visual Studio Code](https://code.visualstudio.com/docs/editing/userdefinedsnippets)