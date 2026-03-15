# Solana - Accounts

An account is Solana's fundamental data unit for storing state. The network stores all state in a key-value store where each key is a 32-byte address and each value is an account.

## 1. Account Structure

### 1.1 Account address

An address can be one of two types:
1. **Public key**: corresponds to an [Ed25519](https://ed25519.cr.yp.to/) keypair (has a private key)
2. **Program derived address (PDA)**: deterministically derived from a program ID and seeds (no private key)
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

The `executable` field determines an account's category:
- **Program accounts**: `executable` = `true`. Contains executable code.
- **Data accounts**: `executable` = `false`. Stores state or user data.

### 2.1 Program accounts

```typescript
import { Connection, PublicKey } from "@solana/web3.js";

const connection = new Connection(
  "https://api.mainnet.solana.com",
  "confirmed"
);

const programId = new PublicKey("TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA");
// Program Account
const accountInfo = await connection.getAccountInfo(programId);
console.log(
  JSON.stringify(
    accountInfo,
    (key, value) => {
      if (key === "data" && value && value.length > 1) {
        return [
          value[0],
          "...truncated, total bytes: " + value.length + "...",
          value[value.length - 1]
        ];
      }
      return value;
    },
    2
  )
);
```

### 2.2 Data accounts

```typescript
import {
  Connection,
  Keypair,
  sendAndConfirmTransaction,
  SystemProgram,
  Transaction,
  LAMPORTS_PER_SOL
} from "@solana/web3.js";
import {
  createInitializeMintInstruction,
  TOKEN_2022_PROGRAM_ID,
  MINT_SIZE,
  getMinimumBalanceForRentExemptMint,
  getMint
} from "@solana/spl-token";

// Create connection to local validator
const connection = new Connection("http://localhost:8899", "confirmed");
const recentBlockhash = await connection.getLatestBlockhash();

// Generate a new keypair for the fee payer
const feePayer = Keypair.generate();

// Airdrop 1 SOL to fee payer
const airdropSignature = await connection.requestAirdrop(
  feePayer.publicKey,
  LAMPORTS_PER_SOL
);
await connection.confirmTransaction({
  blockhash: recentBlockhash.blockhash,
  lastValidBlockHeight: recentBlockhash.lastValidBlockHeight,
  signature: airdropSignature
});

// Generate keypair to use as address of mint
const mint = Keypair.generate();

const createAccountInstruction = SystemProgram.createAccount({
  fromPubkey: feePayer.publicKey,
  newAccountPubkey: mint.publicKey,
  space: MINT_SIZE,
  lamports: await getMinimumBalanceForRentExemptMint(connection),
  programId: TOKEN_2022_PROGRAM_ID
});

const initializeMintInstruction = createInitializeMintInstruction(
  mint.publicKey, // mint pubkey
  9, // decimals
  feePayer.publicKey, // mint authority
  feePayer.publicKey, // freeze authority
  TOKEN_2022_PROGRAM_ID
);

const transaction = new Transaction().add(
  createAccountInstruction,
  initializeMintInstruction
);

const transactionSignature = await sendAndConfirmTransaction(
  connection,
  transaction,
  [feePayer, mint] // Signers
);

console.log("Mint Address: ", mint.publicKey.toBase58());
console.log("Transaction Signature: ", transactionSignature);

// Data Account
const accountInfo = await connection.getAccountInfo(mint.publicKey);
console.log(
  JSON.stringify(
    accountInfo,
    (key, value) => {
      if (key === "data" && value && value.length > 1) {
        return [
          value[0],
          "...truncated, total bytes: " + value.length + "...",
          value[value.length - 1]
        ];
      }
      return value;
    },
    2
  )
);

const mintAccount = await getMint(
  connection,
  mint.publicKey,
  "confirmed",
  TOKEN_2022_PROGRAM_ID
);
console.log(mintAccount);
```

### 2.3 System accounts

```typescript
import { Keypair, Connection, LAMPORTS_PER_SOL } from "@solana/web3.js";

// Generate a new keypair
const keypair = Keypair.generate();
console.log(`Public Key: ${keypair.publicKey}`);

// Create a connection to the Solana cluster
const connection = new Connection("http://localhost:8899", "confirmed");

// Funding an address with SOL automatically creates an account
const signature = await connection.requestAirdrop(
  keypair.publicKey,
  LAMPORTS_PER_SOL
);
await connection.confirmTransaction(signature, "confirmed");

// All wallet accounts are system accounts
const accountInfo = await connection.getAccountInfo(keypair.publicKey);
console.log(JSON.stringify(accountInfo, null, 2));
```

### 2.4 Sysvar accounts

```typescript
import { Connection, SYSVAR_CLOCK_PUBKEY } from "@solana/web3.js";
import { getSysvarClockCodec } from "@solana/sysvars";

const connection = new Connection(
  "https://api.mainnet.solana.com",
  "confirmed"
);

// Sysvar Clock account
const accountInfo = await connection.getAccountInfo(SYSVAR_CLOCK_PUBKEY);

// Deserialize the account data
const decodedClock = getSysvarClockCodec().decode(
  new Uint8Array(accountInfo?.data ?? [])
);

console.log(decodedClock);
console.log(
  JSON.stringify(
    accountInfo,
    (key, value) => {
      if (key === "data" && value && value.length > 1) {
        return [
          value[0],
          "...truncated, total bytes: " + value.length + "...",
          value[value.length - 1]
        ];
      }
      return value;
    },
    2
  )
);
```

#### Reference

- [Accounts](https://solana.com/docs/core/accounts)