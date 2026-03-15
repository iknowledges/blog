# Solana - Instruction

An instruction is a request to execute a specific function on a Solana program.

## 1. Instruction structure

```rust
pub struct Instruction {
    /// Pubkey of the program that executes this instruction.
    pub program_id: Pubkey,
    /// Metadata describing accounts that should be passed to the program.
    pub accounts: Vec<AccountMeta>,
    /// Opaque data passed to the program for its own interpretation.
    pub data: Vec<u8>,
}
```

### Account metadata

```rust
pub struct AccountMeta {
    /// An account's public key.
    pub pubkey: Pubkey,
    /// True if an `Instruction` requires a `Transaction` signature matching `pubkey`.
    pub is_signer: bool,
    /// True if the account data or metadata may be mutated during program execution.
    pub is_writable: bool,
}
```

## 2. Example: SOL transfer instruction

```typescript
import {
  LAMPORTS_PER_SOL,
  SystemProgram,
  Transaction,
  Keypair
} from "@solana/web3.js";

// Generate sender and recipient keypairs
const sender = Keypair.generate();
const recipient = new Keypair();

// Define the amount to transfer
const transferAmount = 0.01; // 0.01 SOL

// Create a transfer instruction for transferring SOL from sender to recipient
const transferInstruction = SystemProgram.transfer({
  fromPubkey: sender.publicKey,
  toPubkey: recipient.publicKey,
  lamports: transferAmount * LAMPORTS_PER_SOL // Convert transferAmount to lamports
});

console.log(JSON.stringify(transferInstruction, null, 2));
```

#### Reference

- [Instruction Structure](https://solana.com/docs/core/instructions/instruction-structure)