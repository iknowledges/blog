# Solana - Accounts

An account is Solana's fundamental data unit for storing state. The network stores all state in a key-value store where each key is a 32-byte address and each value is an account.

## 1. Account Structure

### 1.1 Account address

An account address has two types:

1. Public key
2. Program derived address (PDA)

### 1.2 Account fields

```rust
pub struct Account {
	/// lamports in the account
	pub lamports: u64,
	/// data held in this account
	#[cfg_attr(feature = "serde", serde(with = "serde_bytes"))]
	pub data: Vec<u8>,
	/// the program that owns this account. If executable, the program that loads this account.
	pub owner: Pubkey,
	/// if true, this account's data contains a program (and is now read-only)
	pub executable: bool,
	/// deprecated
	pub rent_epoch: Epoch,
}
```

## 2. Account Type

Account's category:

1. **Program accounts**: `executable` = `true`. Contains executable code.
2. **Data accounts**: `executable` = `false`. Stores state or user data.

### 2.1 Program accounts

Every program account is owned by a loader program.

Program Account:
- Executable: True
- Owner: BPF Loader

#### 2.1.1 Program data accounts

Programs deployed using loader-v3 do not store executable bytecode in `data` field. Instead, their `data` points to a separate **program data account** that contains the program code.

### 2.2 Data accounts

#### 2.2.1 Program state account

Creating a program state account involves two steps:

1. Invoke the System Program to create the account. The System Program transfers ownership to the specified program.
2. The owning program initializes the account's data field.

Data Account:
- Executable: False
- Owner: Program

#### 2.2.2 System accounts

Accounts that remain owned by the System Program after creation are called system accounts. All wallet accounts are system accounts.

Wallet Account:
- Executable: False
- Owner: System Program

#### 2.2.3 Sysvar accounts

Sysvar accounts are special accounts at predefined addresses that provide read-only access to cluster state data.

#### Reference

- [Accounts](https://solana.com/docs/core/accounts)