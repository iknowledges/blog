# uv安装教程

## 安装

### Ubuntu

1. 安装命令：

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

2. 查看uv版本

```
uv self version
```

### Windows

1. 参照[Installer options](https://docs.astral.sh/uv/reference/installer/)，设置环境变量`UV_INSTALL_DIR`修改uv安装路径：

- `UV_INSTALL_DIR`: `D:\Software\uv\install`

2. 然后打开PowerShell执行安装脚本，或者下载[uv-x86_64-pc-windows-msvc.zip](https://github.com/astral-sh/uv/releases)并解压，将`uv.exe`等文件移到`UV_INSTALL_DIR`目录，并将`%UV_INSTALL_DIR%`加入`PATH`：

```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

3. 参考[Storage](https://docs.astral.sh/uv/reference/storage/)，配置如下环境变量，管理缓存储存路径，将`%UV_PYTHON_BIN_DIR%`目录也加入`PATH`：

- `UV_CACHE_DIR`: `D:\Software\uv\cache`
- `UV_PYTHON_INSTALL_DIR`: `D:\Software\uv\python`
- `UV_PYTHON_BIN_DIR`: `D:\Software\uv\bin`
- `UV_TOOL_DIR`: `D:\Software\uv\tools`
- `UV_TOOL_BIN_DIR`: `D:\Software\uv\bin`

4. 使用如下命令查看配置是否生效：

```
uv cache dir
uv python dir
uv python dir --bin
uv tool dir
uv tool dir --bin
```

## 虚拟环境

```
# 创建dev虚拟环境
uv venv dev --seed
# 激活虚拟环境
source dev/bin/activate
# 退出虚拟环境
deactivate
```

--seed: 该选项会在虚拟环境中安装pip，否则在进行包管理时要使用`uv pip`

## 管理python版本

```
# 安装指定版本
uv python install 3.12.3
# 安装并设置为Terminal默认python
uv python install 3.12 --default
# 查看已安装的python
uv python list --only-installed
# 卸载python
uv python uninstall 3.12
```

## 其他命令

```
# 更新uv
uv self update
# 创建项目
uv init example-app
# 添加依赖
uv add httpx
# 移除依赖
uv remove httpx
# 同步依赖
uv sync
# 构建源代码发行版
uv build --sdist
# 构建二进制发行版
uv build --wheel
```

#### 参考资料

- [uv官方文档](https://docs.astral.sh/uv/)
