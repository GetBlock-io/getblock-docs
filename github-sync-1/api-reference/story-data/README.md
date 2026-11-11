---
description: >-
  GetBlock provides fast and reliable access to Story nodes via JSON-RPC API.
  Connect to the Story network without running your own infrastructure.
---

# Story (DATA)

Story is an EVM-compatible Layer-1 blockchain purpose-built for intellectual property: it combines a Cosmos-SDK consensus layer with an Ethereum-equivalent execution layer (story-geth), so standard Solidity contracts and the `eth_*` JSON-RPC surface work alongside Story's native IP-management modules. Tooling such as Foundry, Hardhat, Ethers.js, and Viem works without modification, and the native gas token is DATA. This reference document describes the Ethereum-compatible JSON-RPC interface, served over HTTP.

### Key Features

* **Purpose-Built L1 for IP**: A Cosmos-SDK chain with an EVM execution layer, designed for registering and licensing intellectual property on-chain
* **EVM Compatible**: Runs standard Ethereum bytecode and tooling (Foundry, Hardhat, Ethers, Viem) unchanged
* **DATA Gas**: Transaction fees are paid in DATA, the network's native 18-decimal token
* **EIP-1559 Fees**: Base-fee pricing with `eth_feeHistory` and priority fees, alongside legacy gas
* **Full Node RPC over HTTP**: The complete `eth_*` / `net_*` / `web3_*` surface plus `rpc_modules` and the `debug_*` trace namespace
* **JSON-RPC Interface**: Served over HTTP (this endpoint does not expose WebSocket subscriptions)

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Story implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Story-specific behaviour is documented at_ [_docs.story.foundation_](https://docs.story.foundation/)_._
{% endhint %}

## Network Information

| Property        | Value                            |
| --------------- | -------------------------------- |
| Network Name    | Story                            |
| Chain ID        | 1514                             |
| Native Currency | DATA                             |
| EVM Compatible  | Yes (Cosmos-SDK + EVM execution) |
| Consensus       | Proof-of-Stake (CometBFT)        |
| Fee Model       | EIP-1559 + legacy                |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet | 1514     | ✅        | ❌   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir story-api-quickstart && cd story-api-quickstart && npm init --yes
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
Latest block: 23414734
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
mkdir story-api-quickstart && cd story-api-quickstart
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

| Method                    | Description                                       |
| ------------------------- | ------------------------------------------------- |
| eth\_gasPrice             | Legacy gas price estimate in wei                  |
| eth\_maxPriorityFeePerGas | Suggested EIP-1559 priority fee in wei            |
| eth\_feeHistory           | Historical base fees and priority-fee percentiles |

### Account & State

| Method                   | Description                                   |
| ------------------------ | --------------------------------------------- |
| eth\_getBalance          | ETH balance of an address at a given block    |
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

### RPC Module Discovery

| Method       | Description                                   |
| ------------ | --------------------------------------------- |
| rpc\_modules | List available RPC modules and their versions |

### Debug & Trace

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>debug_accountRange</td><td>Enumerate accounts from the state trie</td></tr><tr><td>debug_batchSendRawTransaction</td><td>Submit multiple signed transactions in one call</td></tr><tr><td>debug_getBadBlocks</td><td>List recently rejected invalid blocks</td></tr><tr><td>debug_storageRangeAt</td><td>Enumerate storage slots of a contract</td></tr><tr><td>debug_traceBlock</td><td>Trace execution of a block by RLP</td></tr><tr><td>debug_traceBlockByHash</td><td>Trace all transactions in a block by hash</td></tr><tr><td>debug_traceBlockByNumber</td><td>Trace all transactions in a block by number</td></tr><tr><td>debug_traceCall</td><td>Trace a simulated message call</td></tr><tr><td>debug_traceTransaction</td><td>Trace execution of a mined transaction</td></tr></tbody></table>

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Story Documentation](https://docs.story.foundation/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Story Explorer](https://www.storyscan.io/)
* [Story Website](https://story.foundation/)
