# Visual Studio Code配置

settings.json可以进行全局设置，也可以在.vscode目录下进行临时设置。

## C/C++插件配置

```json
{
    "C_Cpp.clang_format_style": "Google",
    "C_Cpp.formatting": "vcFormat",
    "[cpp]": {
        "editor.defaultFormatter": "ms-vscode.cpptools",
        "editor.formatOnSave": true
    }
}
```

- clang_format_style: 设置代码格式化的风格，Google风格大括号不换行
- formatting: 设置格式化的引擎
- defaultFormatter: 指定编辑器默认格式化的插件

## CMake Tools插件配置

```json
{
    "cmake.configureArgs": [
        "-DCMAKE_INSTALL_PREFIX=/path/to/prefix",
        "-DTHIRD_PARTY_DIR=/path/to/third_party"
    ],
    "cmake.configureEnvironment": {
        "Python_ROOT": "/path/to/python"
    },
    "cmake.buildArgs": [
        "--verbose"
    ],
    "cmake.debugConfig": {
        "args": [
            "TestArg"
        ]
    },
    "cmake.outputLogEncoding": "utf-8"
}
```

- configureArgs: 执行cmake命令时传入的参数。
- configureEnvironment: 执行cmake命令时的环境变量。
- buildArgs: 执行cmake --build时传入的参数。
- debugConfig: args是调试目标程序时在`int main(int argc, char *argv[])`中传入的参数。
- outputLogEncoding: 设置OUTPUT窗口的字符编码。

## 其他设置

- 设置临时环境变量

```json
{
    "terminal.integrated.env.windows": {
        "PYTHONPATH": "/path/to/library"
    }
}
```

- 解决TERMINAL输出中文乱码，65001为utf-8编码

```json
{
    "terminal.integrated.profiles.windows": {
        "PowerShell": {
            "source": "PowerShell",
            "args": ["-NoExit", "/c", "chcp 65001"],
            "icon": "terminal-powershell"
        },
        "Command Prompt": {
            "path": "cmd.exe",
            "args": ["/K", "chcp 65001"],
            "icon": "terminal-cmd"
        }
    },
    "terminal.integrated.defaultProfile.windows": "Command Prompt"
}
```

对于C/C++程序中出现的中文乱码，也可以在main函数中直接加入如下代码来设置控制台的编码：

```c
int main() {

#if _WIN32 || WIN32
    system("chcp 65001");
#endif

    ......

    return 0;
}
```
