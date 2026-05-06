# Solana常用命令

```
# Run local validator
solana config set -ul
solana-test-validator

# Get current configuration
solana config get
# Change CLI cluster (validators)
solana config set --url mainnet-bata
solana config set --url devnet
solana config set --url localhost
solana config set --url testnet

# Create a wallet
solana-keygen new
# Check wallet address
solana address
# Airdrop SOL (Devnet)
solana config set -ud
solana airdrop 2
# Check balance
solana balance
```

## SPL Token命令

```
spl-token accounts
```

## Anchor命令

```
anchor init test-hello-world
cd test-hello-world
anchor build
anchor test --skip-local-validator
anchor deploy --provoder.cluster localnet
# 更新program id
anchor keys sync
```

- 部署步骤

1. 参照文档[Deploy to Devnet](https://www.anchor-lang.com/docs/quickstart/local)，修改Anchor.toml

```
[provider]
cluster = "Devnet"
wallet = "~/.config/solana/id.json"
```

2. 执行下面命令

```
solana config set --url devnet
anchor deploy -- --use-rpc
```

3. 打开https://explorer.solana.com/搜索`Program Id`

## 问题解决

1. Problem:

```
Caused by:
  failed to parse manifest at `~/.cargo/registry/src/index.crates.io-6f17d22bba15001f/constant_time_eq-0.4.2/Cargo.toml`

Caused by:
  feature `edition2024` is required

  The package requires the Cargo feature called `edition2024`, but that feature is not stabilized in this version of Cargo (1.84.0 (12fe57a9d 2025-04-07)).
  Consider trying a more recent nightly release.
  See https://doc.rust-lang.org/nightly/cargo/reference/unstable.html#edition-2024 for more information about the status of this feature.
```

Solution:

```
$ cargo update -p blake3 --precise 1.8.2
    Updating crates.io index
 Downgrading blake3 v1.8.3 -> v1.8.2
 Downgrading constant_time_eq v0.4.2 -> v0.3.1
note: pass `--verbose` to see 2 unchanged dependencies behind latest
```

2. Problem:

```
Error: AnchorError occurred. Error Code: DeclaredProgramIdMismatch. Error Number: 4100. Error Message: The declared program id does not match the actual program id.
```

Solution:

```
anchor keys sync
```