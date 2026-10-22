---
description: >-
  GetBlock provides fast and reliable access to Citrea nodes via JSON-RPC API.
  Connect to the Citrea network without running your own infrastructure.
---

# Citrea

Citrea is a ZK-rollup that brings EVM smart contracts to Bitcoin: it is the first zkEVM rollup settling to Bitcoin, using BitVM-based bridging and zero-knowledge proofs for verification. Its native gas token is cBTC, a Bitcoin-backed asset.

&#x20;The execution layer is EVM-compatible, so Solidity contracts and the `eth_*` JSON-RPC surface, along with tooling such as Foundry, Hardhat, Ethers.js, and Viem, work unchanged. This reference documents the Ethereum-compatible JSON-RPC interface of the Citrea mainnet, available over HTTP and WebSocket

### Key Features

* **Bitcoin zkEVM Rollup**: The first zkEVM rollup settling to Bitcoin, verified with zero-knowledge proofs
* **cBTC Gas**: Transaction fees are paid in cBTC, a Bitcoin-backed native asset
* **EVM Compatible**: Runs standard Ethereum bytecode and tooling unchanged
* **EIP-1559 Fees**: Base-fee pricing with `eth_feeHistory` and priority fees, alongside legacy gas
* **Conservative RPC Surface**: The widely-supported `eth_*` / `net_*` / `web3_*` methods plus `eth_subscribe` and the core `debug_trace*` methods, over HTTP and WebSocket
* **Testnet**: An environment for building against Citrea before mainnet

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Citrea implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Citrea-specific behavior is documented at_ [_Citrea Documentation_](https://docs.citrea.xyz/)_. Citrea is a zkEVM; some Ethereum methods common on geth nodes may be unavailable._
{% endhint %}

## Network Information

| Property         | Value                 |
| ---------------- | --------------------- |
| Network Name     | Citrea                |
| Chain ID         | 5115                  |
| Native Currency  | cBTC (Bitcoin-backed) |
| EVM Compatible   | Yes (zkEVM)           |
| Settlement Layer | Bitcoin               |
| Fee Model        | EIP-1559 + legacy     |

## Base URL

{% tabs %}
{% tab title="New York, USA" %}
```
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network        | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| -------------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Citrea Mainnet | 4114     | ✅        | ✅   | ❌       | ❌                         | ❌                        | ❌                  | ✅             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir citrea-api-quickstart && cd citrea-api-quickstart && npm init --yes
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

const url = 'https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/';

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
### Response

{% code overflow="wrap" %}
```bash
Latest block: 14270030
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
mkdir citrea-api-quickstart && cd citrea-api-quickstart
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

url = "https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/"

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

| Method              | Description                           |
| ------------------- | ------------------------------------- |
| web3\_clientVersion | Client software version identifier    |
| web3\_sha3          | Keccak-256 hash of hex-encoded data   |
| net\_version        | Network ID as a decimal string        |
| eth\_chainId        | Chain ID per EIP-155                  |
| eth\_blockNumber    | Current chain tip block number        |
| eth\_syncing        | Sync status, or false if fully synced |

### Gas & Fees

| Method                    | Description                                       |
| ------------------------- | ------------------------------------------------- |
| eth\_gasPrice             | Legacy gas price estimate in wei                  |
| eth\_maxPriorityFeePerGas | Suggested EIP-1559 priority fee in wei            |
| eth\_feeHistory           | Historical base fees and priority-fee percentiles |

### Account & State

| Method                   | Description                                 |
| ------------------------ | ------------------------------------------- |
| eth\_getBalance          | GLMR balance of an address at a given block |
| eth\_getStorageAt        | Value at a specific contract storage slot   |
| eth\_getTransactionCount | Address nonce (transaction count)           |
| eth\_getCode             | Deployed bytecode at an address             |

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

### Debug & Trace

| Method                    | Description                                 |
| ------------------------- | ------------------------------------------- |
| debug\_traceBlockByHash   | Trace all transactions in a block by hash   |
| debug\_traceBlockByNumber | Trace all transactions in a block by number |
| debug\_traceCall          | Trace a simulated message call              |
| debug\_traceTransaction   | Trace execution of a mined transaction      |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Citrea Documentation](https://docs.citrea.xyz/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Citrea Explorer](https://explorer.mainnet.citrea.xyz/)
