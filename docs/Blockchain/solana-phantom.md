# Solana CLI关联Phantom钱包

## 方法一

执行下面命令，然后输入助记词，这样本地帐户就会变成Phantom钱包的第一个地址

```
solana-keygen recover 'prompt:?key=0/0' --outfile ~/.config/solana/id.json
```

## 方法二

1. 安装python包：

```
pip install base58
```

2. 使用下面的命令将Phantoom私钥转成byte数组，并替换`~/.config/solana/id.json`的内容：

```
>>> import base58
>>> [i for i in base58.b58decode('<PRIVATE_KEY_BASE58>')]
```

3. 查看帐户地址是否和Phantom钱包一致：

```
solana address
```

#### 参考资料

- [Using a Phantom Wallet Address with the Solana CLI](https://mattmazur.com/2021/11/18/using-a-phantom-wallet-address-with-the-solana-cli/)
- [How do I export a wallet to a json file from a wallet provider like phantom or solflare to use in the Solana CLI?](https://solana.stackexchange.com/questions/1018/how-do-i-export-a-wallet-to-a-json-file-from-a-wallet-provider-like-phantom-or-s)
