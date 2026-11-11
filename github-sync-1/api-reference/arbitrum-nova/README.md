---
description: >-
  GetBlock provides fast and reliable access to Arbitrum Nova nodes via JSON-RPC
  API. Connect to the Arbitrum Nova network without running your own
  infrastructure.
---

# Arbitrum Nova

Arbitrum Nova is an EVM-compatible Layer 2 built on the Arbitrum Nitro stack. Unlike a full rollup, Nova is an AnyTrust chain: transaction data availability is attested by a Data Availability Committee rather than posted in full to Ethereum L1, which lowers fees and suits high-throughput applications such as gaming and social. It settles to Ethereum L1 and secures execution with interactive fraud proofs, falling back to rollup-style data posting if the committee is unavailable. Because Nova is EVM-compatible, standard Ethereum contracts, JSON-RPC methods, and tooling such as Foundry, Hardhat, Ethers.js, and Viem work without modification, alongside Arbitrum precompiles such as ArbSys and ArbGasInfo. Native gas is paid in ETH.

### Key Features

* **EVM Compatibility**: Standard Ethereum contracts, JSON-RPC, and tooling run unchanged on the Nitro stack
* **AnyTrust Data Availability**: A Data Availability Committee attests to data, cutting costs versus full-rollup calldata posting
* **Interactive Fraud Proofs**: Execution is secured by Nitro's fraud-proof system with settlement on Ethereum L1
* **Arbitrum Precompiles**: Chain-specific precompiles including ArbSys (`0x64`), ArbGasInfo (`0x6C`), and NodeInterface
* **Ethereum Tooling**: Compatible with MetaMask, Foundry, Hardhat, Remix, Ethers.js, and Viem
* **ETH Gas**: Native gas is paid in ETH; the fee includes an L2 execution component and an L1 data component

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Arbitrum Nova implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Arbitrum-specific behavior is documented at_ [_docs.arbitrum.io_](https://docs.arbitrum.io/)_._
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Arbitrum Nova</td></tr><tr><td>Chain ID</td><td>42170</td></tr><tr><td>Native Currency</td><td>ETH</td></tr><tr><td>EVM Compatible</td><td>Yes (Arbitrum Nitro)</td></tr><tr><td>Data Availability</td><td>AnyTrust (Data Availability Committee)</td></tr><tr><td>Settlement Layer</td><td>Ethereum L1</td></tr><tr><td>Block Time</td><td>Sub-second (~0.25s)</td></tr><tr><td>Finality</td><td>L1-backed after the challenge window</td></tr></tbody></table>

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Nova    | 42170    | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir arbitrum-nova-quickstart && cd arbitrum-nova-quickstart && npm init --yes
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
{
    "jsonrpc": "2.0",
    "result": "0x51528e0",
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
mkdir arbitrum-nova-quickstart && cd arbitrum-nova-quickstart
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

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>web3_clientVersion</td><td>Client software version identifier</td></tr><tr><td>web3_sha3</td><td>Keccak-256 hash of hex-encoded data</td></tr><tr><td>net_version</td><td>Network ID as a decimal string</td></tr><tr><td>eth_chainId</td><td>Chain ID per EIP-155</td></tr><tr><td>eth_blockNumber</td><td>Current chain tip block number</td></tr><tr><td>eth_syncing</td><td>Sync status, or false if fully synced</td></tr></tbody></table>

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

* [Arbitrum Documentation](https://docs.arbitrum.io/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Arbitrum Nova Explorer (Arbiscan)](https://nova.arbiscan.io/)
* [Arbitrum Bridge](https://bridge.arbitrum.io/)
* [Arbitrum Website](https://arbitrum.io/)
