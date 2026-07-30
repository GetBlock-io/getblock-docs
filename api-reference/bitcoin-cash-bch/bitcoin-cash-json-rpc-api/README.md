---
description: >-
  GetBlock provides fast and reliable access to Bitcoin Cash nodes via JSON-RPC
  API. Connect to the Bitcoin Cash network without running your own
  infrastructure.
---

# Bitcoin Cash JSON-RPC API

The Bitcoin Cash (BCH) JSON-RPC API provides high-performance, indexed blockchain data via JSON methods. This allows you to perform complex, fast queries—such as fetching address transaction histories, wallet balances, xpub states, UTXOs, and real-time fiat conversion rates

### Base URL

```bash
https://go.getblock.io/
```

### Available API Methods

GetBlock provides access to Bitcoin Cash Node JSON-RPC methods. Methods that require wallet or node-administration privileges are not available on shared endpoints and are omitted below.

#### Blockchain Methods

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>getbestblockhash</td><td>Returns the hash of the best (tip) block</td></tr><tr><td>getblock</td><td>Returns block data by hash</td></tr><tr><td>getblockchaininfo</td><td>Returns blockchain state information</td></tr><tr><td>getblockcount</td><td>Returns the height of the chain tip</td></tr><tr><td>getblockhash</td><td>Returns the block hash at a height</td></tr><tr><td>getblockheader</td><td>Returns a block header by hash</td></tr><tr><td>getblockstats</td><td>Returns per-block statistics</td></tr><tr><td>getchaintips</td><td>Returns known chain tips</td></tr><tr><td>getchaintxstats</td><td>Returns chain-wide transaction statistics</td></tr><tr><td>getdifficulty</td><td>Returns the current difficulty</td></tr><tr><td>getfinalizedblockhash</td><td>Returns the finalized block hash</td></tr><tr><td>gettxout</td><td>Returns an unspent output</td></tr><tr><td>gettxoutproof</td><td>Returns a transaction inclusion proof</td></tr><tr><td>preciousblock</td><td>Marks a block as preferred during a tie</td></tr></tbody></table>

#### Mempool Methods

| Method                | Description                     |
| --------------------- | ------------------------------- |
| getrawmempool         | Returns mempool transaction ids |
| getmempoolinfo        | Returns mempool state           |
| getmempoolentry       | Returns a mempool entry         |
| getmempoolancestors   | Returns in-mempool ancestors    |
| getmempooldescendants | Returns in-mempool descendants  |

#### Transaction Methods

| Method                    | Description                          |
| ------------------------- | ------------------------------------ |
| getrawtransaction         | Returns raw transaction data         |
| sendrawtransaction        | Broadcasts a signed transaction      |
| createrawtransaction      | Builds an unsigned transaction       |
| decoderawtransaction      | Decodes raw transaction hex          |
| decodescript              | Decodes a hex script                 |
| combinerawtransaction     | Merges partially signed transactions |
| signrawtransactionwithkey | Signs a transaction with keys        |
| testmempoolaccept         | Tests mempool acceptance             |

#### PSBT Methods

| Method        | Description                        |
| ------------- | ---------------------------------- |
| createpsbt    | Creates an empty PSBT              |
| converttopsbt | Converts a raw transaction to PSBT |
| combinepsbt   | Combines PSBTs                     |
| decodepsbt    | Decodes a PSBT                     |
| finalizepsbt  | Finalizes a PSBT                   |

#### Mining Methods

| Method           | Description                         |
| ---------------- | ----------------------------------- |
| getmininginfo    | Returns mining information          |
| getnetworkhashps | Estimates network hash rate         |
| getblocktemplate | Returns a block template for mining |

#### Utility Methods

| Method          | Description                      |
| --------------- | -------------------------------- |
| getmemoryinfo   | Returns node memory usage        |
| getrpcinfo      | Returns RPC server runtime state |
| uptime          | Returns node uptime              |
| validateaddress | Validates a BCH address          |
| verifymessage   | Verifies a signed message        |
| help            | Lists commands or command help   |
