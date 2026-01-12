# Solana开发环境安装

首先要安装好Rust，这里略过了。

## 安装Solana CLI

1. 安装 Solana CLI 工具套件：

```
sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"
```

2. 添加PATH环境变量

```
export PATH="/Users/test/.local/share/solana/install/active_release/bin:$PATH"
```

3. 更新环境变量

```
source ~/.bashrc
```

4. 检查版本

```
solana --version
```

5. 根据需要将 Solana CLI 更新到最新版本 (可选)

```
agave-install update
```

## 安装Anchor CLI

前置条件：安装Node和Yarn

```
nvm install node
npm install --global yarn
```

#### 直接安装

1. 使用以下命令安装特定版本的Anchor CLI

```
cargo install --git https://github.com/solana-foundation/anchor --tag v0.30.1 anchor-cli
```

2. 验证安装是否成功

```
anchor --version
```

#### 通过AVM安装Anchor CLI

1. 安装Anchor Version Manager (AVM)

```
cargo install --git https://github.com/solana-foundation/anchor avm --force
```

3. 确认 AVM 是否成功安装：

```
avm --version
```

4. 使用 AVM 安装 Anchor CLI：

```
# 安装最新版本
avm install latest
avm use latest
# 安装特定版本
avm install 0.30.1
avm use 0.30.1
```

5. 验证安装是否成功

```
anchor --version
```

#### 参考资料

- [Solana 文档 - 快速安装](https://solana.com/zh/docs/intro/installation)