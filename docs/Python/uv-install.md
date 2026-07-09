# uv安装教程

## 安装

1. 安装命令：

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

2. 查看uv版本

```
uv self version
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
# 查看已安装的python
uv python list --only-installed
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
```

#### 参考资料

- [uv官方文档](https://docs.astral.sh/uv/)
