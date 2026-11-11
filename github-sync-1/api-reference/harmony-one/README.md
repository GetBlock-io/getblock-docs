---
description: >-
  GetBlock provides fast and reliable access to Harmony nodes via JSON-RPC API.
  Connect to the Harmony network without running your own infrastructure.
---

# Harmony (ONE)

Harmony is an EVM-compatible, sharded Layer 1 blockchain secured by Effective Proof-of-Stake (EPoS) and Fast Byzantine Fault Tolerant (FBFT) consensus, which gives it roughly two-second blocks and fast, deterministic finality. Its beacon shard, Shard 0, runs an Ethereum-compatible execution layer: standard Ethereum contracts, the `eth_*` JSON-RPC surface, and tooling such as Foundry, Hardhat, Ethers.js, and Viem work directly against it. The native token is ONE. Harmony also exposes native `hmy_*` / `hmyv2_*` RPC methods and a dual address format (a bech32 `one1...` form alongside the Ethereum `0x` form); this reference covers the Ethereum-compatible `eth_*` surface on Shard 0.

### Key Features

* **EVM Compatibility**: Shard 0 runs standard Ethereum contracts, JSON-RPC, and tooling unchanged
* **Sharded Layer 1**: A beacon shard (Shard 0) plus additional shards, secured natively rather than by another chain
* **EPoS + FBFT Consensus**: Effective Proof-of-Stake with FBFT gives fast, deterministic finality and no probabilistic reorgs
* **ONE Gas Token**: Native gas is paid in ONE; Harmony uses legacy gas pricing, not EIP-1559 base fees
* **Dual Addressing**: Accounts have a bech32 `one1...` form and an Ethereum `0x` form; the `eth_*` API uses the `0x` form
* **Ethereum Tooling**: Compatible with MetaMask, Foundry, Hardhat, Remix, Ethers.js, and Viem

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Harmony's Shard 0 implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Harmony-specific behavior and native methods are documented at_ [_docs.harmony.one_](https://docs.harmony.one/)_._
{% endhint %}

## Network Information

| Property        | Value                             |
| --------------- | --------------------------------- |
| Network Name    | Harmony                           |
| Chain ID        | 1666600000 (Shard 0)              |
| Native Currency | ONE                               |
| EVM Compatible  | Yes (Shard 0)                     |
| Consensus       | Effective Proof-of-Stake + FBFT   |
| Block Time      | \~2 seconds                       |
| Finality        | Fast, deterministic (\~2 seconds) |
| Gas Pricing     | Legacy (no EIP-1559 base fee)     |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network           | Chain ID   | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ----------------- | ---------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet (Shard 0) | 1666600000 | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir harmony-api-quickstart && cd harmony-api-quickstart && npm init --yes
```
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'eth_blockNumber',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  console.log('Latest block:', parseInt(response.data.result, 16));
})
.catch(error => console.error(error));
```
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```
{% endstep %}

{% step %}
### Reponse

{% code overflow="wrap" %}
```bash
{
    "jsonrpc": "2.0",
    "result": "0x5907ed0",
    "id": "getblock.io"
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir harmony-api-quickstart && cd harmony-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endstep %}

{% step %}
### Install dependency

```bash
pip install requests
```
{% endstep %}

{% step %}
### Add code

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
})

headers = {
    'Content-Type': 'application/json'
}

response = requests.post(url, headers=headers, data=payload)
print(response.text)
```
{% endcode %}
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}
{% endtab %}
{% endtabs %}

## Available API Methods

### Chain & Client Info

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>web3_clientVersion</td><td>Client software version identifier</td></tr><tr><td>web3_sha3</td><td>Keccak-256 hash of hex-encoded data</td></tr><tr><td>net_version</td><td>Network ID as a decimal string</td></tr><tr><td>net_listening</td><td>Whether the node is listening for P2P connections</td></tr><tr><td>net_peerCount</td><td>Number of connected peers</td></tr><tr><td>eth_chainId</td><td>Chain ID per EIP-155</td></tr><tr><td>eth_blockNumber</td><td>Current chain tip block number</td></tr><tr><td>eth_syncing</td><td>Sync status, or false if fully synced</td></tr></tbody></table>

### Gas & Fees

| Method        | Description                      |
| ------------- | -------------------------------- |
| eth\_gasPrice | Legacy gas price estimate in wei |

### Account & State

| Method                   | Description                                |
| ------------------------ | ------------------------------------------ |
| eth\_getBalance          | ONE balance of an address at a given block |
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

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Harmony Documentation](https://docs.harmony.one/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Harmony Block Explorer](https://explorer.harmony.one/)
* [Harmony Bridge](https://bridge.harmony.one/)
* [Harmony Website](https://harmony.one/)
