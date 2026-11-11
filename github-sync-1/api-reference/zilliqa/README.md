---
description: >-
  GetBlock provides fast and reliable access to Zilliqa nodes via JSON-RPC API.
  Connect to the Zilliqa network without running your own infrastructure.
---

# Zilliqa

Zilliqa is a sharded Layer-1 blockchain whose 2.0 upgrade adds an Ethereum-compatible execution layer alongside its original Scilla smart-contract platform.&#x20;

This reference document describes the **EVM** JSON-RPC interface (the `eth_*` surface), so standard Solidity contracts and tooling such as Foundry, Hardhat, Ethers.js, and Viem work against it.&#x20;

The native gas token is ZIL. The API is available over HTTP and WebSocket. Zilliqa's separate native (Scilla) API is a distinct interface and is not covered here.

### Key Features

* **EVM on a Sharded L1**: Zilliqa 2.0 runs an Ethereum-compatible execution layer on its sharded Layer-1
* **EVM Compatible**: Standard Solidity contracts and tooling (Foundry, Hardhat, Ethers, Viem) run unchanged
* **ZIL Gas**: Transaction fees are paid in ZIL
* **EIP-1559 Fees**: Base-fee pricing with `eth_feeHistory` and priority fees, alongside legacy gas
* **Dual Smart-Contract Platforms**: EVM (Solidity) and the original Scilla run side by side; this reference covers the EVM `eth_*` API
* _eth\_ JSON-RPC_\*: The widely-supported Ethereum method surface over HTTP and WebSocket

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Zilliqa implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Zilliqa-specific behavior is documented at_ [_Zilliqa 2.0 Documentation_](https://dev.zilliqa.com/zilliqa2)_. Zilliqa also exposes a separate native (Scilla) API that is not covered here._
{% endhint %}

## Network Information

| Property        | Value                 |
| --------------- | --------------------- |
| Network Name    | Zilliqa Mainnet (EVM) |
| Chain ID        | 32769                 |
| Native Currency | ZIL                   |
| EVM Compatible  | Yes (Zilliqa 2.0)     |
| Consensus       | Sharded PoS (pBFT)    |
| Fee Model       | EIP-1559 + legacy     |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Zilliqa is served from the **Frankfurt** region only — use the Frankfurt base URL above. WebSocket is available at `wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Supported Networks

| Network         | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| --------------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Zilliqa Mainnet | 32769    | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir zilliqa-api-quickstart && cd zilliqa-api-quickstart && npm init --yes
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
### Response

{% code overflow="wrap" %}
```
Latest block: 
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
mkdir zilliqa-api-quickstart && cd zilliqa-api-quickstart
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
| eth\_getBalance          | ZIL balance of an address at a given block |
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

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Zilliqa 2.0 Documentation](https://dev.zilliqa.com/zilliqa2)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Zilliqa Explorer](https://zilliqa.blockscout.com/)
