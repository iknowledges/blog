# Foundry开发教程

## 安装

```
curl -L https://foundry.paradigm.xyz | bash
source ~/.bashrc
foundryup
```

## 卸载

先执行下面命令，然后删除`~/.bashrc`中有关Foundry PATH的行

```
rm -rf ~/.foundry
```

## forge常用命令

```
# 创建项目
forge init hello_foundry
# 编译合约
forge build
# 测试合约
forge test
# 使用脚本部署合约
forge script script/Counter.s.sol
# 部署合约
forge create src/Counter.sol:Counter --rpc-url http://127.0.0.1:8545   --interactive --broadcast
```

## anvil常用命令

```
# 启动本地网络节点
anvil
```

## cast常用命令

```
# 转账
cast send 0x70997970C51812dc3A010C7d01b50e0d17dc79C8 \
    --value 1ether \
    --private-key 0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```

#### 参考资料

- [Foundry](https://www.getfoundry.sh/)