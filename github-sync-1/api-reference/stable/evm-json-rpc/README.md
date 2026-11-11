---
description: >-
  GetBlock provides fast and reliable access to Stable nodes via the EVM
  JSON-RPC API. Connect to the Stable network without running your own
  infrastructure.
---

# EVM JSON RPC

The Ethereum-compatible JSON-RPC interface for Stable, served by its EVM execution layer: contract calls and simulation, account and block reads, transaction submission, filters and logs, WebSocket subscriptions, and `debug_*` tracing. Methods follow the standard Ethereum JSON-RPC specification, so Foundry, Hardhat, Ethers.js, Viem, and MetaMask work unchanged against chain ID 988. Gas is paid in USDT0.

## Endpoints

{% tabs %}
{% tab title="HTTP" %}
{% code overflow="wrap" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}
{% endtab %}

{% tab title="WebSocket" %}
{% code overflow="wrap" %}
```bash
wss://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. HTTP and WebSocket are provisioned as separate endpoints — select the API interface when you create the endpoint. Stable is served from the Singapore region.
{% endhint %}

## Available API Methods

### Chain & Client Info

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td><code>web3_clientVersion</code></td><td>Client software version identifier</td></tr><tr><td><code>web3_sha3</code></td><td>Keccak-256 hash of hex-encoded data</td></tr><tr><td><code>net_version</code></td><td>Network ID as a decimal string</td></tr><tr><td><code>net_listening</code></td><td>Whether the node is listening for P2P connections</td></tr><tr><td><code>net_peerCount</code></td><td>Number of connected peers</td></tr><tr><td><code>eth_chainId</code></td><td>Chain ID per EIP-155</td></tr><tr><td><code>eth_blockNumber</code></td><td>Current chain tip block number</td></tr><tr><td><code>eth_syncing</code></td><td>Sync status, or false if fully synced</td></tr></tbody></table>

### Gas & Fees

| Method                     | Description                                       |
| -------------------------- | ------------------------------------------------- |
| `eth_gasPrice`             | Legacy gas price estimate in wei                  |
| `eth_maxPriorityFeePerGas` | Suggested EIP-1559 priority fee in wei            |
| `eth_feeHistory`           | Historical base fees and priority-fee percentiles |

### Account & State

| Method                    | Description                                  |
| ------------------------- | -------------------------------------------- |
| `eth_getBalance`          | USDT0 balance of an address at a given block |
| `eth_accounts`            | Accounts owned by the client                 |
| `eth_getStorageAt`        | Value at a specific contract storage slot    |
| `eth_getTransactionCount` | Address nonce (transaction count)            |
| `eth_getCode`             | Deployed bytecode at an address              |

### Blocks

| Method                                 | Description                             |
| -------------------------------------- | --------------------------------------- |
| `eth_getBlockByHash`                   | Block data by block hash                |
| `eth_getBlockByNumber`                 | Block data by block number or tag       |
| `eth_getBlockTransactionCountByHash`   | Transaction count in a block, by hash   |
| `eth_getBlockTransactionCountByNumber` | Transaction count in a block, by number |

### Transactions

| Method                                    | Description                                    |
| ----------------------------------------- | ---------------------------------------------- |
| `eth_getTransactionByHash`                | Transaction data by transaction hash           |
| `eth_getTransactionByBlockHashAndIndex`   | Transaction by block hash and index            |
| `eth_getTransactionByBlockNumberAndIndex` | Transaction by block number and index          |
| `eth_getTransactionReceipt`               | Transaction receipt with status, gas, and logs |

### Execution & Simulation

| Method            | Description                                    |
| ----------------- | ---------------------------------------------- |
| `eth_call`        | Execute a message call without a transaction   |
| `eth_estimateGas` | Estimate gas required to execute a transaction |

### Filters & Logs

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td><code>eth_newFilter</code></td><td>Create a log filter with address and topic criteria</td></tr><tr><td><code>eth_newBlockFilter</code></td><td>Create a filter for new block hashes</td></tr><tr><td><code>eth_newPendingTransactionFilter</code></td><td>Create a filter for pending transaction hashes</td></tr><tr><td><code>eth_uninstallFilter</code></td><td>Remove a previously created filter</td></tr><tr><td><code>eth_getFilterChanges</code></td><td>Poll new entries for a filter since the last poll</td></tr><tr><td><code>eth_getFilterLogs</code></td><td>Get all logs matching a log filter</td></tr><tr><td><code>eth_getLogs</code></td><td>Query logs matching criteria in one call</td></tr></tbody></table>

### Transaction Submission

| Method                   | Description                                |
| ------------------------ | ------------------------------------------ |
| `eth_sendRawTransaction` | Submit a signed transaction to the network |

### WebSocket Subscriptions

| Method            | Description                                                     |
| ----------------- | --------------------------------------------------------------- |
| `eth_subscribe`   | Subscribe to newHeads, logs, newPendingTransactions, or syncing |
| `eth_unsubscribe` | Cancel an active subscription                                   |

### Debug & Trace

| Method                     | Description                                 |
| -------------------------- | ------------------------------------------- |
| `debug_traceBlockByHash`   | Trace all transactions in a block by hash   |
| `debug_traceBlockByNumber` | Trace all transactions in a block by number |
| `debug_traceCall`          | Trace a simulated message call              |
| `debug_traceTransaction`   | Trace execution of a mined transaction      |

## Stable-specific behavior

Stable runs an EVM execution layer on a CometBFT (StableBFT) consensus engine, which changes a few assumptions carried over from Ethereum mainnet:

| Topic          | What to expect on Stable                                                                                                                      |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Finality       | CometBFT gives deterministic single-block finality, so a result read at `latest` is already final and not subject to probabilistic reorgs     |
| Gas token      | Gas is paid in USDT0. As the native gas token it uses 18 decimals (wei-style); the USDT0 ERC-20 token uses 6 decimals — keep the two straight |
| Subscriptions  | `eth_subscribe` and `eth_unsubscribe` work only over the `wss://` endpoint; calling them over HTTP returns error `-32601`                     |
| Method surface | This is the widely-supported cross-client set; geth-only methods (`eth_getProof`, `rpc_modules`, geth `debug_*` extras) are not included      |

## See Also

* [Stable overview](../) — network information, interfaces, and quickstart
* [Deploy a Smart Contract on Stable](deploy-a-smart-contract-on-stable.md)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
