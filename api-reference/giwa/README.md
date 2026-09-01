---
description: >-
  GetBlock provides fast and reliable access to GIWA nodes via JSON-RPC API.
  Connect to the GIWA network without running your own infrastructure.
---

# GIWA

GIWA is an EVM-compatible Layer 2 built on the OP Stack as an optimistic rollup, developed by Dunamu — the operator of the Upbit exchange — together with the Optimism Foundation. \
\
GIWA Sepolia is its public test network, connected to Ethereum Sepolia for settlement and bridging, where developers validate contracts and integrations before mainnet. The network targets roughly one-second block times and supports Flashblocks preconfirmations for faster user-facing feedback.&#x20;

Because GIWA is EVM-equivalent, standard Ethereum JSON-RPC methods and tooling such as Foundry, Hardhat, Ethers.js, and Viem work without modification. Native gas is paid in ETH.

### Key Features

* **OP Stack Layer 2**: Optimistic rollup built on the OP Stack, posting data to and settling on Ethereum
* **Sub-Second Blocks**: Approximately one-second block times for fast transaction inclusion
* **Flashblocks Preconfirmations**: A Flashblocks-aware endpoint streams preconfirmations (\~200 ms) ahead of full block production
* **EVM Equivalence**: Runs standard Solidity and EVM bytecode unmodified, with the full Ethereum JSON-RPC surface
* **ETH Gas Token**: Transactions are paid in ETH with 18 decimals
* **Ethereum-Backed Security**: Inherits data availability and settlement guarantees from Ethereum Sepolia
* **Deterministic Predeploys**: Standard OP Stack predeploys at fixed `0x42...` addresses (WETH, GasPriceOracle, bridge contracts)
* **Ethereum Tooling**: Compatible with MetaMask, Foundry, Hardhat, Remix, Ethers.js, and Viem

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>GIWA</td></tr><tr><td>Chain ID</td><td>91342</td></tr><tr><td>Native Currency</td><td>ETH</td></tr><tr><td>Decimals</td><td>18</td></tr><tr><td>Block Time</td><td>~1 second</td></tr><tr><td>Consensus</td><td>Optimistic Rollup (OP Stack)</td></tr><tr><td>EVM Compatible</td><td>Yes</td></tr></tbody></table>

## Base URL

{% tabs %}
{% tab title="Singapore, Singapore" %}
```bash
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | Singapore, Singapore |
| ------- | -------- | -------- | --- | -------------------- |
| Sepolia | 91342    | ✅        | ✅   | ✅                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir giwa-api-quickstart && cd giwa-api-quickstart && npm init --yes
```
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Create file

```bash
touch index.js
```
{% endstep %}

{% step %}
### Set ES module type

Add `"type": "module"` to `package.json`.
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'eth_chainId',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  const blockNumber = parseInt(response.data.result, 16);
  console.log('Current Block Number:', blockNumber);
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
### Result

{% code overflow="wrap" %}
```bash
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x164ce"
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
mkdir giwa-api-quickstart && cd giwa-api-quickstart
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
### Create file

```bash
touch main.py
```
{% endstep %}

{% step %}
### Add code

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "eth_chainId",
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
| eth\_blobBaseFee          | Current blob base fee for EIP-4844 transactions   |
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
| eth\_simulateV1       | Simulate a bundle of transactions against a block |

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

* [GIWA Documentation](https://docs.giwa.io)
* [GIWA Explorer (Blockscout)](https://sepolia-explorer.giwa.io)
* [GIWA Website](https://giwa.io)
* [Ethereum JSON-RPC Specification](https://ethereum.org/developers/docs/apis/json-rpc/)
