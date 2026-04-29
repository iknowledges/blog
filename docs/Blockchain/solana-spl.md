# Solana - SPL Tokens

## 1. Token Programs

Token Programs contains all instruction logic for interacting with tokens on the network.

## 2. Token

All tokens on Solana are data accounts owned by a Token Program.

### 2.1 Mint Account

Mint Account acts as a global counter for a specific token and stores data such as:

- Supply: Total supply of the token
- Decimals: Decimal precision of the token
- Mint authority: The account authorized to create new units of the token
- Freeze authority: The account authorized to freeze tokens

### 2.2 Token Account

Token Account tracks individual ownership of each token unit and stores data such as:

- Mint: The token that Token Account holds units of
- Owner: The account authorized to transfer tokens from the Token Account
- Amount: Number of the tokens the Token Account currently holds

### 2.3 Associated Token Account

Associated Token Accounts simplify the process of finding a token account's address for a specific mint and owner.

It's important to understand that an Associated Token Account is just a token account with a specific address (usually PDA).

## 3. Token CLI Examples

### 3.1 Create Mint Account

```
spl-token create-token
# Check the current supply
spl-token supply <TOKEN_ADDRESS>
```

### 3.2 Create Token Account

```
spl-token create-account <TOKEN_ADDRESS>
# To create a token account with a different owner
spl-token create-account --owner <OWNER_ADDRESS> <TOKEN_ADDRESS>
```

Example command:

```
spl-token create-account 99zqUzQGohamfYxyo8ykTEbi91iom3CLmwCA75FK5zTg
```

### 3.3 Mint Tokens

```
spl-token mint [OPTIONS] <TOKEN_ADDRESS> <TOKEN_AMOUNT> [--] [RECIPIENT_TOKEN_ACCOUNT_ADDRESS]
```

Example command:

```
spl-token mint 99zqUzQGohamfYxyo8ykTEbi91iom3CLmwCA75FK5zTg 100
```

### 3.4 Transfer Tokens

```
spl-token transfer [OPTIONS] <TOKEN_ADDRESS> <TOKEN_AMOUNT> <RECIPIENT_ADDRESS or RECIPIENT_TOKEN_ACCOUNT_ADDRESS>
```

Example command:

```
spl-token transfer 99zqUzQGohamfYxyo8ykTEbi91iom3CLmwCA75FK5zTg 100 Hmyk3FSw4cfsuAes7sanp2oxSkE9ivaH6pMzDzbacqmt
```

#### Reference

- [Tokens on Solana](https://solana.com/docs/tokens)