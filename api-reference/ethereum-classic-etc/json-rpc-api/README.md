# JSON-RPC API

## Available API Methods

### Chain & Client Info

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>web3_clientVersion</td><td>Client software version identifier</td></tr><tr><td>web3_sha3</td><td>Keccak-256 hash of hex-encoded data</td></tr><tr><td>net_version</td><td>Network ID as a decimal string</td></tr><tr><td>net_listening</td><td>Whether the node is listening for P2P connections</td></tr><tr><td>net_peerCount</td><td>Number of connected peers</td></tr><tr><td>eth_chainId</td><td>Chain ID per EIP-155</td></tr><tr><td>eth_blockNumber</td><td>Current chain tip block number</td></tr><tr><td>eth_syncing</td><td>Sync status, or false if fully synced</td></tr></tbody></table>

### Gas & Fees

| Method        | Description                      |
| ------------- | -------------------------------- |
| eth\_gasPrice | Legacy gas price estimate in wei |

### Account & State

| Method                   | Description                                   |
| ------------------------ | --------------------------------------------- |
| eth\_getBalance          | ETC balance of an address at a given block    |
| eth\_accounts            | Accounts owned by the client                  |
| eth\_getStorageAt        | Value at a specific contract storage slot     |
| eth\_getTransactionCount | Address nonce (transaction count)             |
| eth\_getCode             | Deployed bytecode at an address               |
| eth\_getProof            | Merkle-Patricia proof for account and storage |

### Blocks

| Method                                | Description                             |
| ------------------------------------- | --------------------------------------- |
| eth\_getBlockByHash                   | Block data by block hash                |
| eth\_getBlockByNumber                 | Block data by block number or tag       |
| eth\_getBlockTransactionCountByHash   | Transaction count in a block, by hash   |
| eth\_getBlockTransactionCountByNumber | Transaction count in a block, by number |
| eth\_getBlockReceipts                 | All transaction receipts for a block    |

### Transactions

| Method                                   | Description                                    |
| ---------------------------------------- | ---------------------------------------------- |
| eth\_getTransactionByHash                | Transaction data by transaction hash           |
| eth\_getTransactionByBlockHashAndIndex   | Transaction by block hash and index            |
| eth\_getTransactionByBlockNumberAndIndex | Transaction by block number and index          |
| eth\_getTransactionReceipt               | Transaction receipt with status, gas, and logs |

### Execution & Simulation

| Method                | Description                                       |
| --------------------- | ------------------------------------------------- |
| eth\_call             | Execute a message call without a transaction      |
| eth\_estimateGas      | Estimate gas required to execute a transaction    |
| eth\_createAccessList | Compute an EIP-2930 access list for a transaction |

### Filters & Logs

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>eth_newFilter</td><td>Create a log filter with address and topic criteria</td></tr><tr><td>eth_newBlockFilter</td><td>Create a filter for new block hashes</td></tr><tr><td>eth_newPendingTransactionFilter</td><td>Create a filter for pending transaction hashes</td></tr><tr><td>eth_uninstallFilter</td><td>Remove a previously created filter</td></tr><tr><td>eth_getFilterChanges</td><td>Poll new entries for a filter since the last poll</td></tr><tr><td>eth_getFilterLogs</td><td>Get all logs matching a log filter</td></tr><tr><td>eth_getLogs</td><td>Query logs matching criteria in one call</td></tr></tbody></table>

### Transaction Submission

| Method                  | Description                                |
| ----------------------- | ------------------------------------------ |
| eth\_sendRawTransaction | Submit a signed transaction to the network |

### WebSocket Subscriptions

| Method           | Description                                                     |
| ---------------- | --------------------------------------------------------------- |
| eth\_subscribe   | Subscribe to newHeads, logs, newPendingTransactions, or syncing |
| eth\_unsubscribe | Cancel an active subscription                                   |

### RPC Module Discovery

| Method       | Description                                   |
| ------------ | --------------------------------------------- |
| rpc\_modules | List available RPC modules and their versions |

### Debug & Trace

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>debug_accountRange</td><td>Enumerate accounts from the state trie</td></tr><tr><td>debug_batchSendRawTransaction</td><td>Submit multiple signed transactions in one call</td></tr><tr><td>debug_getBadBlocks</td><td>List recently rejected invalid blocks</td></tr><tr><td>debug_storageRangeAt</td><td>Enumerate storage slots of a contract</td></tr><tr><td>debug_traceBlock</td><td>Trace execution of a block by RLP</td></tr><tr><td>debug_traceBlockByHash</td><td>Trace all transactions in a block by hash</td></tr><tr><td>debug_traceBlockByNumber</td><td>Trace all transactions in a block by number</td></tr><tr><td>debug_traceCall</td><td>Trace a simulated message call</td></tr><tr><td>debug_traceTransaction</td><td>Trace execution of a mined transaction</td></tr></tbody></table>

### Mining

| Method          | Description                            |
| --------------- | -------------------------------------- |
| eth\_mining     | Whether the node is actively mining    |
| eth\_hashrate   | Node mining hashrate                   |
| eth\_coinbase   | Client coinbase (miner reward) address |
| eth\_getWork    | Current PoW work package               |
| eth\_submitWork | Submit a Proof-of-Work solution        |
