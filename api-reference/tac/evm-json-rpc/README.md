---
description: >-
  GetBlock provides fast and reliable access to TAC nodes via the EVM JSON-RPC
  API. Connect to the TAC network without running your own infrastructure.
---

# EVM JSON RPC

The Ethereum-compatible JSON-RPC interface for TAC, served by the cosmos/evm execution layer: contract calls and simulation, account and block reads, transaction submission, filters and logs, WebSocket subscriptions, and `debug_*` tracing. Methods follow the standard Ethereum JSON-RPC specification, so Foundry, Hardhat, Ethers.js, Viem, and MetaMask work unchanged against chain ID 239 (mainnet) or 2391 (SPB testnet).

## Endpoints

{% tabs %}
{% tab title="HTTP" %}
{% code overflow="wrap" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}
{% endtab %}

{% tab title="WebSocket" %}
{% code overflow="wrap" %}
```bash
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. HTTP and WebSocket are provisioned as separate endpoints — select the API interface when you create the endpoint. On the SPB testnet, use the Frankfurt endpoint.
{% endhint %}

## Available API Methods

### Chain & Client Info

| Method                                                                        | Description                                       |
| ----------------------------------------------------------------------------- | ------------------------------------------------- |
| [web3\_clientVersion](/broken/pages/ec10414766293baf9b4ddae1350209894c82a95e) | Client software version identifier                |
| [web3\_sha3](/broken/pages/d93a36250e91a3f5ef82b025cb2dbfebef08d051)          | Keccak-256 hash of hex-encoded data               |
| [net\_version](/broken/pages/d69789af0e13ede1bd5597de1444a51fcc620a55)        | Network ID as a decimal string                    |
| [net\_listening](/broken/pages/6fffc2b7268197dd8aeef52b971b3c9728ca3dfd)      | Whether the node is listening for P2P connections |
| [net\_peerCount](/broken/pages/3f7c2bac12a382819e7cd277078487262ebb9457)      | Number of connected peers                         |
| [eth\_chainId](/broken/pages/403787363d4b882407c03d4a73365d4b0bf328f3)        | Chain ID per EIP-155                              |
| [eth\_blockNumber](/broken/pages/c667c653ddf9fee76ca37669e397754f9345e128)    | Current chain tip block number                    |
| [eth\_syncing](/broken/pages/233035fa59cc91fb9885c1931154a85e24fd87cc)        | Sync status, or false if fully synced             |

### Gas & Fees

| Method                                                                              | Description                                       |
| ----------------------------------------------------------------------------------- | ------------------------------------------------- |
| [eth\_gasPrice](/broken/pages/9d822055240ffa7a7d4b44be17d293b583d74ed7)             | Legacy gas price estimate in wei                  |
| [eth\_maxPriorityFeePerGas](/broken/pages/3a1bfe605e0cd7c51548b1164a1e26756d3fdc09) | Suggested EIP-1559 priority fee in wei            |
| [eth\_feeHistory](/broken/pages/565af3d5794924b44ba3086698a151536707974f)           | Historical base fees and priority-fee percentiles |

### Account & State

| Method                                                                             | Description                                |
| ---------------------------------------------------------------------------------- | ------------------------------------------ |
| [eth\_getBalance](/broken/pages/015173ba6ee0946a68f83bd48f66f0399c62025c)          | TAC balance of an address at a given block |
| [eth\_accounts](/broken/pages/5c1ef90140c6bdd48d3f9d1418e7448c31a06434)            | Accounts owned by the client               |
| [eth\_getStorageAt](/broken/pages/56a913f6584189eff0bebfae8d2889bf1711f9db)        | Value at a specific contract storage slot  |
| [eth\_getTransactionCount](/broken/pages/9471c3df9bb8fcd7d94efe2c49fc7c1f5e979ec0) | Address nonce (transaction count)          |
| [eth\_getCode](/broken/pages/4561c25ce9c5583e282870a45ccf1f2572699f25)             | Deployed bytecode at an address            |

### Blocks

| Method                                                                                          | Description                             |
| ----------------------------------------------------------------------------------------------- | --------------------------------------- |
| [eth\_getBlockByHash](/broken/pages/f6412a7bac5894bc0c76575954b830d9e5ed48bd)                   | Block data by block hash                |
| [eth\_getBlockByNumber](/broken/pages/6bf710c5c6bc72820cb30737720bec170b056dac)                 | Block data by block number or tag       |
| [eth\_getBlockTransactionCountByHash](/broken/pages/3f5cb685a7d6b6c0f3877e3d2b1030c7ff55db21)   | Transaction count in a block, by hash   |
| [eth\_getBlockTransactionCountByNumber](/broken/pages/803518e2b412104681617cc5716ae2db7c23c82a) | Transaction count in a block, by number |

### Transactions

| Method                                                                                             | Description                                    |
| -------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| [eth\_getTransactionByHash](/broken/pages/395c853fb3c40f16ed9c57f782c960f52410443d)                | Transaction data by transaction hash           |
| [eth\_getTransactionByBlockHashAndIndex](/broken/pages/ec0614b24b64eeb6d4dd322f741de2a8f827e963)   | Transaction by block hash and index            |
| [eth\_getTransactionByBlockNumberAndIndex](/broken/pages/30bf7f4eaf399ca2cf79d48a84eaa793935defb3) | Transaction by block number and index          |
| [eth\_getTransactionReceipt](/broken/pages/c92f5ba77cb4251b7265800a4407ca23487fe41c)               | Transaction receipt with status, gas, and logs |

### Execution & Simulation

| Method                                                                     | Description                                    |
| -------------------------------------------------------------------------- | ---------------------------------------------- |
| [eth\_call](/broken/pages/469a4cf26b044a07677e641b34395697503212ca)        | Execute a message call without a transaction   |
| [eth\_estimateGas](/broken/pages/af2aee2f4c476b2f744b80b2ae7cb16f92163207) | Estimate gas required to execute a transaction |

### Filters & Logs

| Method                                                                                     | Description                                         |
| ------------------------------------------------------------------------------------------ | --------------------------------------------------- |
| [eth\_newFilter](/broken/pages/c20d498988d2f6215d491fe4245b9cd2cbe5560a)                   | Create a log filter with address and topic criteria |
| [eth\_newBlockFilter](/broken/pages/47ed685e13774d286c6c81bc0a2bb184467dd8b2)              | Create a filter for new block hashes                |
| [eth\_newPendingTransactionFilter](/broken/pages/1424fea1034289eb638301ba1796258baccad0f1) | Create a filter for pending transaction hashes      |
| [eth\_uninstallFilter](/broken/pages/e18f941e1858ba6686c093ddba1097057f0a0f38)             | Remove a previously created filter                  |
| [eth\_getFilterChanges](/broken/pages/54cd0bc68f178d95a552dfb7db57230cd01304e7)            | Poll new entries for a filter since the last poll   |
| [eth\_getFilterLogs](/broken/pages/9b400b5e5c529afc5bf15cce2bf9e94f5e0c8296)               | Get all logs matching a log filter                  |
| [eth\_getLogs](/broken/pages/642036ef5de25ad499cbd55a7a31f4dc40785d42)                     | Query logs matching criteria in one call            |

### Transaction Submission

| Method                                                                            | Description                                |
| --------------------------------------------------------------------------------- | ------------------------------------------ |
| [eth\_sendRawTransaction](/broken/pages/11bd17285716cd82c621e8066c6e2e459df12631) | Submit a signed transaction to the network |

### WebSocket Subscriptions

| Method                                                                     | Description                                                     |
| -------------------------------------------------------------------------- | --------------------------------------------------------------- |
| [eth\_subscribe](/broken/pages/2993ea8ba7fe25080295fc2efcfcd47fd93a9033)   | Subscribe to newHeads, logs, newPendingTransactions, or syncing |
| [eth\_unsubscribe](/broken/pages/e0eb63ed426b70889689951ff4306d8b195d85bc) | Cancel an active subscription                                   |

### Debug & Trace

| Method                                                                              | Description                                 |
| ----------------------------------------------------------------------------------- | ------------------------------------------- |
| [debug\_traceBlockByHash](/broken/pages/fd4704e2303a2786d4aa079758ad32b9397f90e8)   | Trace all transactions in a block by hash   |
| [debug\_traceBlockByNumber](/broken/pages/37be60bfd1deb722287498b519dd488f6905ee28) | Trace all transactions in a block by number |
| [debug\_traceCall](/broken/pages/dd56b2c9a7c103a8d9e96441f093694b3e85a4f5)          | Trace a simulated message call              |
| [debug\_traceTransaction](/broken/pages/3c2207e400d17fd6f0a6ce4a5300c4ed3bcadc45)   | Trace execution of a mined transaction      |

## TAC-specific behaviour

TAC runs a standard Ethereum execution layer (cosmos/evm) on a Cosmos SDK chain, which changes a few assumptions carried over from Ethereum mainnet:

| Topic         | What to expect on TAC                                                                                                                                               |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Finality      | CometBFT gives deterministic single-block finality, so a result read at `latest` is already final and not subject to probabilistic reorgs                           |
| Fees          | EIP-1559 pricing comes from the feemarket module — `eth_gasPrice`, `eth_maxPriorityFeePerGas`, and `eth_feeHistory` all apply, and gas is paid in TAC (18 decimals) |
| Subscriptions | `eth_subscribe` and `eth_unsubscribe` work only over the `wss://` endpoint; calling them over HTTP returns error `-32601`                                           |
| Addresses     | An account's `0x…` and bech32 `tac1…` forms are two encodings of the same key — the `eth_*` methods here take the `0x` form                                         |
| Networks      | TAC Mainnet is chain ID 239 (`0xef`); the Saint Petersburg (SPB) testnet is chain ID 2391 (`0x957`)                                                                 |

## See Also

* [TAC overview](/broken/pages/3d68f19c295c4bf9aebed430b6e7dd2f2a109fb2) — network information, interfaces, and quickstart
* [Deploy a Smart Contract on TAC](/broken/pages/1a1ab7c35801f81a5f17f79c9a18aaab33d61ef2)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [TAC Documentation](https://docs.tac.build/)
