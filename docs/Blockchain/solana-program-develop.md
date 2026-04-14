# Solana Program开发教程

## 快速开始

1. 创建项目

```
cargo new hello_world --lib
```

修改Cargo.toml中edition为2021：

```
[package]
name = "hello_world"
version = "0.1.0"
edition = "2021"

[dependencies]
```

2. 添加依赖

```
cargo add solana-program@2.2.0
```

3. 修改Cargo.toml添加crate-type

```
[package]
name = "hello_world"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib", "lib"]

[dependencies]
solana-program = "2.2.0"
```

4. 编写lib.rs：

```rust
use solana_program::{
    account_info::AccountInfo, entrypoint, entrypoint::ProgramResult, msg, pubkey::Pubkey,
};

entrypoint!(process_instruction);

pub fn process_instruction(
    _program_id: &Pubkey,
    _accounts: &[AccountInfo],
    _instruction_data: &[u8],
) -> ProgramResult {
    msg!("Hello, world!");
    Ok(())
}
```

5. 编译程序

```
cargo build-sbf
# 查看program ID
solana address -k ./target/deploy/hello_world-keypair.json
```

## 测试程序

1. 添加测试依赖

```
cargo add litesvm@0.6.1 --dev
cargo add solana-sdk@2.2.0 --dev
```

2. 修改lib.rs，编写测试代码：

```rust
use solana_program::{
    account_info::AccountInfo, entrypoint, entrypoint::ProgramResult, msg, pubkey::Pubkey,
};

entrypoint!(process_instruction);

pub fn process_instruction(
    _program_id: &Pubkey,
    _accounts: &[AccountInfo],
    _instruction_data: &[u8],
) -> ProgramResult {
    msg!("Hello, world!");
    Ok(())
}

#[cfg(test)]
mod test {
    use litesvm::LiteSVM;
    use solana_sdk::{
        instruction::Instruction,
        message::Message,
        signature::{Keypair, Signer},
        transaction::Transaction,
    };

    #[test]
    fn test_hello_world() {
        // Create a new LiteSVM instance
        let mut svm = LiteSVM::new();

        // Create a keypair for the transaction payer
        let payer = Keypair::new();

        // Airdrop some lamports to the payer
        svm.airdrop(&payer.pubkey(), 1_000_000_000).unwrap();

        // Load our program
        let program_keypair = Keypair::new();
        let program_id = program_keypair.pubkey();
        svm.add_program_from_file(program_id, "target/deploy/hello_world.so")
            .unwrap();

        // Create instruction with no accounts and no data
        let instruction = Instruction {
            program_id,
            accounts: vec![],
            data: vec![],
        };

        // Create transaction
        let message = Message::new(&[instruction], Some(&payer.pubkey()));
        let transaction = Transaction::new(&[&payer], message, svm.latest_blockhash());

        // Send transaction and verify it succeeds
        let result = svm.send_transaction(transaction);
        assert!(result.is_ok(), "Transaction should succeed");
        let logs = result.unwrap().logs;
        println!("Logs: {logs:#?}");
    }
}
```

3. 测试程序

```
cargo test -- --no-capture
```

## 部署程序

1. 启动本地测试网

```
solana-test-validator
```

2. 部署合约

```
solana config set -ul
solana program deploy ./target/deploy/hello_world.so
```

## 创建客户端程序

1. 创建client.rs

```
mkdir -p examples && touch examples/client.rs
```

2. 修改Cargo.toml，添加client.rs

```
[[example]]
name = "client"
path = "examples/client.rs"
```

3. 添加依赖

```
cargo add solana-client@2.2.0 --dev
cargo add tokio --dev
```

4. 编写client.rs：

```rust
use solana_client::rpc_client::RpcClient;
use solana_sdk::{
    commitment_config::CommitmentConfig,
    instruction::Instruction,
    pubkey::Pubkey,
    signature::{Keypair, Signer},
    transaction::Transaction,
};
use std::str::FromStr;

#[tokio::main]
async fn main() {
    // Program ID (replace with your actual program ID)
    let program_id = Pubkey::from_str("4Ujf5fXfLx2PAwRqcECCLtgDxHKPznoJpa43jUBxFfMz").unwrap();

    // Connect to the Solana devnet
    let rpc_url = String::from("http://localhost:8899");
    let client = RpcClient::new_with_commitment(rpc_url, CommitmentConfig::confirmed());

    // Generate a new keypair for the payer
    let payer = Keypair::new();

    // Request airdrop
    let airdrop_amount = 1_000_000_000; // 1 SOL
    let signature = client
        .request_airdrop(&payer.pubkey(), airdrop_amount)
        .expect("Failed to request airdrop");

    // Wait for airdrop confirmation
    loop {
        let confirmed = client.confirm_transaction(&signature).unwrap();
        if confirmed {
            break;
        }
    }

    // Create the instruction
    let instruction = Instruction::new_with_borsh(
        program_id,
        &(),    // Empty instruction data
        vec![], // No accounts needed
    );

    // Add the instruction to new transaction
    let mut transaction = Transaction::new_with_payer(&[instruction], Some(&payer.pubkey()));
    transaction.sign(&[&payer], client.get_latest_blockhash().unwrap());

    // Send and confirm the transaction
    match client.send_and_confirm_transaction(&transaction) {
        Ok(signature) => println!("Transaction Signature: {}", signature),
        Err(err) => eprintln!("Error sending transaction: {}", err),
    }
}
```

5. 运行客户端程序：

```
cargo run --example client
```

## 其他命令

```
# 关闭程序
solana program close 4Ujf5fXfLx2PAwRqcECCLtgDxHKPznoJpa43jUBxFfMz --bypass-warning
# 重新部署关闭的程序
solana-keygen new -o ./target/deploy/hello_world-keypair.json --force
```

#### 参考资料

- [Developing Programs in Rust](https://solana.com/docs/programs/rust)