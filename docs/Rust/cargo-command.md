# 常用cargo命令

## 发布crates包

1. 使用github账号登录[crates.io](https://crates.io)，找到【Account Settings】->【API Tokens】并创建一个token。
2. 在Cargo.toml中填写好description、license和repository等信息后，使用下面命令进行发布：

```
cargo login <your_token>
cargo publish
cargo logout
```