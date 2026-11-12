---
description: >-
  GetBlock provides fast and reliable access to Elysium nodes via JSON-RPC API.
  Connect to the Elysium network without running your own infrastructure.
---

# Elysium

Elysium is a general-purpose, high-performance Layer-2 for Hyperliquid, built by Kinetiq. It is an Arbitrum Orbit chain running Arbitrum Nitro, settles to HyperEVM, and targets \~100ms blocks for low-latency applications.

Because it runs a standard Ethereum execution layer, Solidity contracts and the `eth_*` JSON-RPC surface, along with tooling such as Foundry, Hardhat, Ethers.js, and Viem, work unchanged. The native gas token is HYPE, bridged 1:1 from HyperEVM. This reference describes the Ethereum-compatible JSON-RPC interface, available over HTTP and WebSocket.

### Key Features

* **Arbitrum Orbit L2**: An Arbitrum Nitro chain for Hyperliquid that settles to HyperEVM
* **\~100ms Blocks**: Low-latency block production for trading and real-time applications
* **EVM Equivalent**: Runs standard Ethereum bytecode and tooling (Foundry, Hardhat, Ethers, Viem) unchanged
* **HYPE Gas**: Transaction fees are paid in HYPE, bridged 1:1 from HyperEVM (18 decimals)
* **Full Node RPC**: The complete `eth_*` / `net_*` / `web3_*` surface plus `rpc_modules` and the `debug_*` trace namespace, over HTTP and WebSocket
* **EIP-1559 Fees**: Base-fee pricing with `eth_feeHistory` and priority fees; Nitro adds the cost of posting the transaction to the parent chain

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Elysium implements the standard Ethereum JSON-RPC interface via Arbitrum Nitro; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Elysium-specific behavior is documented at_ [_elysium.kinetiq.xyz/docs_](https://elysium.kinetiq.xyz/docs)_._
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Elysium Mainnet</td></tr><tr><td>Chain ID</td><td>99901 (0x1863d)</td></tr><tr><td>Native Currency</td><td>HYPE (bridged 1:1 from HyperEVM)</td></tr><tr><td>EVM Compatible</td><td>Yes (Arbitrum Nitro)</td></tr><tr><td>Settlement Layer</td><td>HyperEVM</td></tr><tr><td>Developer</td><td>Kinetiq</td></tr><tr><td>Fee Model</td><td>EIP-1559 + legacy, plus Nitro parent-chain posting cost</td></tr></tbody></table>

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Elysium is served from the **Frankfurt** region only — use the Frankfurt base URL above. WebSocket is available at `wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet | 99901    | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir elysium-api-quickstart && cd elysium-api-quickstart && npm init --yes
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
Latest block: 5257974
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
mkdir elysium-api-quickstart && cd elysium-api-quickstart
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

### Available API Methods

#### Chain & Client Info

| Method              | Description                           |
| ------------------- | ------------------------------------- |
| web3\_clientVersion | Client software version identifier    |
| web3\_sha3          | Keccak-256 hash of hex-encoded data   |
| net\_version        | Network ID as a decimal string        |
| eth\_chainId        | Chain ID per EIP-155                  |
| eth\_blockNumber    | Current chain tip block number        |
| eth\_syncing        | Sync status, or false if fully synced |

#### Gas & Fees

| Method                    | Description                                       |
| ------------------------- | ------------------------------------------------- |
| eth\_gasPrice             | Legacy gas price estimate in wei                  |
| eth\_maxPriorityFeePerGas | Suggested EIP-1559 priority fee in wei            |
| eth\_feeHistory           | Historical base fees and priority-fee percentiles |

#### Account & State

| Method                   | Description                                   |
| ------------------------ | --------------------------------------------- |
| eth\_getBalance          | HYPE balance of an address at a given block   |
| eth\_accounts            | Accounts owned by the client                  |
| eth\_getStorageAt        | Value at a specific contract storage slot     |
| eth\_getTransactionCount | Address nonce (transaction count)             |
| eth\_getCode             | Deployed bytecode at an address               |
| eth\_getProof            | Merkle-Patricia proof for account and storage |

#### Blocks

| Method                                | Description                             |
| ------------------------------------- | --------------------------------------- |
| eth\_getBlockByHash                   | Block data by block hash                |
| eth\_getBlockByNumber                 | Block data by block number or tag       |
| eth\_getBlockTransactionCountByHash   | Transaction count in a block, by hash   |
| eth\_getBlockTransactionCountByNumber | Transaction count in a block, by number |
| eth\_getBlockReceipts                 | All transaction receipts for a block    |

#### Transactions

| Method                                   | Description                                    |
| ---------------------------------------- | ---------------------------------------------- |
| eth\_getTransactionByHash                | Transaction data by transaction hash           |
| eth\_getTransactionByBlockHashAndIndex   | Transaction by block hash and index            |
| eth\_getTransactionByBlockNumberAndIndex | Transaction by block number and index          |
| eth\_getTransactionReceipt               | Transaction receipt with status, gas, and logs |

#### Execution & Simulation

| Method                | Description                                       |
| --------------------- | ------------------------------------------------- |
| eth\_call             | Execute a message call without a transaction      |
| eth\_estimateGas      | Estimate gas required to execute a transaction    |
| eth\_createAccessList | Compute an EIP-2930 access list for a transaction |

#### Filters & Logs

| Method                           | Description                                         |
| -------------------------------- | --------------------------------------------------- |
| eth\_newFilter                   | Create a log filter with address and topic criteria |
| eth\_newBlockFilter              | Create a filter for new block hashes                |
| eth\_newPendingTransactionFilter | Create a filter for pending transaction hashes      |
| eth\_uninstallFilter             | Remove a previously created filter                  |
| eth\_getFilterChanges            | Poll new entries for a filter since the last poll   |
| eth\_getFilterLogs               | Get all logs matching a log filter                  |
| eth\_getLogs                     | Query logs matching criteria in one call            |

#### Transaction Submission

| Method                  | Description                                |
| ----------------------- | ------------------------------------------ |
| eth\_sendRawTransaction | Submit a signed transaction to the network |

#### WebSocket Subscriptions

| Method           | Description                                                     |
| ---------------- | --------------------------------------------------------------- |
| eth\_subscribe   | Subscribe to newHeads, logs, newPendingTransactions, or syncing |
| eth\_unsubscribe | Cancel an active subscription                                   |

#### RPC Module Discovery

| Method       | Description                                   |
| ------------ | --------------------------------------------- |
| rpc\_modules | List available RPC modules and their versions |

#### Debug & Trace

| Method                         | Description                                     |
| ------------------------------ | ----------------------------------------------- |
| debug\_accountRange            | Enumerate accounts from the state trie          |
| debug\_batchSendRawTransaction | Submit multiple signed transactions in one call |
| debug\_getBadBlocks            | List recently rejected invalid blocks           |
| debug\_storageRangeAt          | Enumerate storage slots of a contract           |
| debug\_traceBlock              | Trace execution of a block by RLP               |
| debug\_traceBlockByHash        | Trace all transactions in a block by hash       |
| debug\_traceBlockByNumber      | Trace all transactions in a block by number     |
| debug\_traceCall               | Trace a simulated message call                  |
| debug\_traceTransaction        | Trace execution of a mined transaction          |

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [Elysium Documentation](https://elysium.kinetiq.xyz/docs)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Arbitrum Nitro Documentation](https://docs.arbitrum.io)
* [Kinetiq](https://kinetiq.xyz)
