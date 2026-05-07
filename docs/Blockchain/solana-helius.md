# Solana使用Helius RPC节点

1. 访问[Devnet RPC URL](https://dashboard.helius.dev/endpoints)获取API Key，然后修改Devnet的RPC URL：

```
solana config set -ud
solana config set --url https://devnet.helius-rpc.com/?api-key=<your-api-key>
```

2. 查看余额

```
solana balance
```

## JS中用法

```javascript
import { Connection, LAMPORTS_PER_SOL, PublicKey } from '@solana/web3.js'

const apiKey = process.env.HELIUS_API_KEY;
let endpoint = `https://devnet.helius-rpc.com/?api-key=${apiKey}`;
const connection = new Connection(endpoint, "confirmed");

const publicKey = new PublicKey('YOUR_PUBLIC_KEY');
const balance = await connection.getBalance(publicKey);
console.log(`Balance: ${balance / LAMPORTS_PER_SOL} SOL`);
```

#### 参考资料

- [How to Get Devnet SOL](https://www.helius.dev/docs/rpc/devnet-sol)