---
description: >-
  JSON-RPC API reference for the Cronos EVM. Explore method list, request
  examples, and how to connect to GetBlock's Cronos RPC endpoints
---

# EVM JSON RPC - Cronos

The Ethereum-compatible JSON-RPC interface for Cronos, served by the Ethermint execution layer: contract calls and simulation, account and block reads, transaction submission, filters and logs, WebSocket subscriptions, and `debug_*` tracing. Methods follow the standard Ethereum JSON-RPC specification, so Foundry, Hardhat, Ethers.js, Viem, and MetaMask work unchanged against chain ID 25.

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
Replace `<ACCESS-TOKEN>` with the access token from the GetBlock dashboard. HTTP and WebSocket are provisioned as separate endpoints — select the API interface when you create the endpoint.
{% endhint %}

## Available API Methods

### Chain & Client Info

| Method              | Description                                       |
| ------------------- | ------------------------------------------------- |
| web3\_clientVersion | Client software version identifier                |
| web3\_sha3          | Keccak-256 hash of hex-encoded data               |
| net\_version        | Network ID as a decimal string                    |
| net\_listening      | Whether the node is listening for P2P connections |
| net\_peerCount      | Number of connected peers                         |
| eth\_chainId        | Chain ID per EIP-155                              |
| eth\_blockNumber    | Current chain tip block number                    |
| eth\_syncing        | Sync status, or false if fully synced             |

### Gas & Fees

| Method                    | Description                                       |
| ------------------------- | ------------------------------------------------- |
| eth\_gasPrice             | Legacy gas price estimate in wei                  |
| eth\_maxPriorityFeePerGas | Suggested EIP-1559 priority fee in wei            |
| eth\_feeHistory           | Historical base fees and priority-fee percentiles |

### Account & State

| Method                   | Description                                |
| ------------------------ | ------------------------------------------ |
| eth\_getBalance          | CRO balance of an address at a given block |
| eth\_accounts            | Accounts owned by the client               |
| eth\_getStorageAt        | Value at a specific contract storage slot  |
| eth\_getTransactionCount | Address nonce (transaction count)          |
| eth\_getCode             | Deployed bytecode at an address            |

### Blocks

| Method                                | Description                             |
| ------------------------------------- | --------------------------------------- |
| eth\_getBlockByHash                   | Block data by block hash                |
| eth\_getBlockByNumber                 | Block data by block number or tag       |
| eth\_getBlockTransactionCountByHash   | Transaction count in a block, by hash   |
| eth\_getBlockTransactionCountByNumber | Transaction count in a block, by number |

### Transactions

| Method                                   | Description                                    |
| ---------------------------------------- | ---------------------------------------------- |
| eth\_getTransactionByHash                | Transaction data by transaction hash           |
| eth\_getTransactionByBlockHashAndIndex   | Transaction by block hash and index            |
| eth\_getTransactionByBlockNumberAndIndex | Transaction by block number and index          |
| eth\_getTransactionReceipt               | Transaction receipt with status, gas, and logs |

### Execution & Simulation

| Method           | Description                                    |
| ---------------- | ---------------------------------------------- |
| eth\_call        | Execute a message call without a transaction   |
| eth\_estimateGas | Estimate gas required to execute a transaction |

### Filters & Logs

| Method                           | Description                                         |
| -------------------------------- | --------------------------------------------------- |
| eth\_newFilter                   | Create a log filter with address and topic criteria |
| eth\_newBlockFilter              | Create a filter for new block hashes                |
| eth\_newPendingTransactionFilter | Create a filter for pending transaction hashes      |
| eth\_uninstallFilter             | Remove a previously created filter                  |
| eth\_getFilterChanges            | Poll new entries for a filter since the last poll   |
| eth\_getFilterLogs               | Get all logs matching a log filter                  |
| eth\_getLogs                     | Query logs matching criteria in one call            |

### Transaction Submission

| Method                  | Description                                |
| ----------------------- | ------------------------------------------ |
| eth\_sendRawTransaction | Submit a signed transaction to the network |

### WebSocket Subscriptions

| Method           | Description                                                     |
| ---------------- | --------------------------------------------------------------- |
| eth\_subscribe   | Subscribe to newHeads, logs, newPendingTransactions, or syncing |
| eth\_unsubscribe | Cancel an active subscription                                   |

### Debug & Trace

| Method                    | Description                                 |
| ------------------------- | ------------------------------------------- |
| debug\_traceBlockByHash   | Trace all transactions in a block by hash   |
| debug\_traceBlockByNumber | Trace all transactions in a block by number |
| debug\_traceCall          | Trace a simulated message call              |
| debug\_traceTransaction   | Trace execution of a mined transaction      |

## Cronos-specific behaviour

Cronos runs a standard Ethereum execution layer on a Cosmos SDK chain, which changes a few assumptions carried over from Ethereum mainnet:

| Topic | What to expect on Cronos |
| ----- | ------------------------ |
| Finality | CometBFT gives deterministic single-block finality, so a result read at `latest` is already final and is not subject to probabilistic reorgs |
| Fees | EIP-1559 pricing comes from Ethermint's feemarket module — `eth_gasPrice`, `eth_maxPriorityFeePerGas`, and `eth_feeHistory` all apply, and gas is paid in CRO (18 decimals) |
| Subscriptions | `eth_subscribe` and `eth_unsubscribe` work only over the `wss://` endpoint; calling them over HTTP returns error `-32601` |
| Addresses | An account's `0x…` and bech32 `crc1…` forms are two encodings of the same key — the `eth_*` methods here take the `0x` form |
| Chain identity | This is Cronos EVM (chain ID 25), which is a different network from Cronos zkEVM (chain ID 388) |

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Cronos implements the standard Ethereum JSON-RPC interface via Ethermint; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Cronos-specific behaviour is documented at_ [_docs.cronos.org_](https://docs.cronos.org/)_._
{% endhint %}

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Cronos (CRO) overview](../) — network information, interfaces, and quickstart
* [Deploy a Smart Contract on Cronos](deploy-a-smart-contract-on-cronos.md)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Official Cronos Documentation](https://docs.cronos.org/)
