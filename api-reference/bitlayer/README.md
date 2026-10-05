---
description: >-
  GetBlock provides fast and reliable access to Bitlayer nodes via JSON-RPC API.
  Connect to the Bitlayer network without running your own infrastructure.
---

# Bitlayer

Bitlayer is a Bitcoin Layer-2 network that brings an EVM execution environment to Bitcoin, letting Ethereum-style smart contracts settle with Bitcoin-anchored security. It is EVM-compatible — standard Solidity contracts, the `eth_*` JSON-RPC surface, and tooling such as Foundry, Hardhat, Ethers.js, and Viem work without modification — while the native gas token is BTC rather than ETH. This reference documents the Ethereum-compatible JSON-RPC interface Bitlayer exposes, available over HTTP and WebSocket.

Because Bitlayer is a Bitcoin L2 rather than an Ethereum-mainnet client, this reference documents the methods its nodes serve in practice: the core `eth_*` / `net_*` / `web3_*` read and write surface, EIP-1559 fee methods, filters and subscriptions, and the core `debug_trace*` tracers. Ethereum-mainnet-specific methods that do not apply to a Bitcoin L2 — notably `eth_blobBaseFee` (EIP-4844 blobs), `eth_getProof`, `eth_createAccessList`, and `eth_simulateV1` — are intentionally not listed.

### Key Features

* **Bitcoin Security**: An L2 that anchors to Bitcoin, pairing EVM programmability with Bitcoin-grade settlement
* **EVM Compatible**: Runs standard Ethereum bytecode and tooling (Foundry, Hardhat, Ethers, Viem) unmodified
* **BTC as Gas**: Transaction fees are paid in BTC, the network's native 18-decimal gas token
* **EIP-1559 Fees**: Base-fee pricing with `eth_feeHistory` and priority fees
* **Full Read/Write RPC**: Account, block, transaction, log, and filter methods over HTTP and WebSocket
* **WebSocket Subscriptions**: Live `eth_subscribe` streams for new heads, logs, and pending transactions

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Bitlayer implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Bitlayer-specific behaviour is documented at_ [_docs.bitlayer.org_](https://docs.bitlayer.org/)_._
{% endhint %}

## Network Information

| Property        | Value           |
| --------------- | --------------- |
| Network Name    | Bitlayer        |
| Chain ID        | 200901          |
| Native Currency | BTC             |
| EVM Compatible  | Yes             |
| Network Type    | Bitcoin Layer-2 |
| Fee Model       | EIP-1559        |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet | 200901   | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ✅             | ✅                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir bitlayer-api-quickstart && cd bitlayer-api-quickstart && npm init --yes
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
```bash
Latest Block: 26307046
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
mkdir bitlayer-api-quickstart && cd bitlayer-api-quickstart
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
| eth\_getBalance          | BTC balance of an address at a given block |
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

* [Official Bitlayer Documentation](https://docs.bitlayer.org/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [BTR Scan Explorer](https://www.btrscan.com/)
* [Bitlayer Website](https://www.bitlayer.org/)
