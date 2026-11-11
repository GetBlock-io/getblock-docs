---
description: >-
  GetBlock provides fast and reliable access to Lens nodes via JSON-RPC API.
  Connect to the Lens network without running your own infrastructure.
---

# Lens (GHO)

Lens (Lens Chain) is a ZKsync Era / ZK Stack Layer-2 purpose-built for the Lens social protocol. It runs the ZK Stack's EraVM, so it is EVM-compatible rather than EVM-identical: the standard `eth_*` JSON-RPC surface and tooling such as Ethers.js and Viem work for reads and transactions, while deployment uses the ZKsync `zksolc` compiler. Native gas is paid in GHO, the Aave-ecosystem stablecoin. Alongside the Ethereum-compatible methods, Lens exposes the ZKsync `zks_` namespace for L1/L2 messaging, batch details, and fee estimation. This reference documents the interface over HTTP and WebSocket.

### Key Features

* **ZK Stack Layer 2**: A ZKsync Era chain settling to Ethereum with validity proofs
* **Built for Lens Protocol**: The home chain for the Lens decentralized social graph
* **GHO Gas**: Transaction fees are paid in GHO, the native gas token
* **EraVM**: Contracts compile with `zksolc`; plain `solc` bytecode is not used directly
* **zks\_ Namespace**: ZKsync-specific methods for batches, fees, bridges, and L1/L2 proofs
* **WebSocket Subscriptions**: Live `eth_subscribe` streams alongside the full read/write surface

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Lens implements the standard Ethereum JSON-RPC interface plus the ZKsync `zks_` extensions; the canonical specifications are the Ethereum JSON-RPC spec at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/) _and the ZKsync JSON-RPC docs at_ [_docs.zksync.io_](https://docs.zksync.io/)_. Lens-specific guidance is at_ [_lens.xyz_](https://lens.xyz/)_._
{% endhint %}

## Network Information

| Property        | Value                        |
| --------------- | ---------------------------- |
| Network Name    | Lens Chain                   |
| Chain ID        | 232                          |
| Native Currency | GHO                          |
| EVM Compatible  | Yes (ZK Stack EraVM, zksolc) |
| Consensus       | ZK Rollup (ZKsync Era)       |
| Fee Model       | ZKsync fee model             |

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
| Mainnet | 232      | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir lens-api-quickstart && cd lens-api-quickstart && npm init --yes
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
Latest block: 6349051
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
mkdir lens-api-quickstart && cd lens-api-quickstart
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

| Method                   | Description                                |
| ------------------------ | ------------------------------------------ |
| eth\_getBalance          | GHO balance of an address at a given block |
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

### zkSync Stack Extensions (zks)

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>zks_L1BatchNumber</td><td>Latest L1 batch number</td></tr><tr><td>zks_getL1BatchDetails</td><td>Details of an L1 batch</td></tr><tr><td>zks_getL1BatchBlockRange</td><td>L2 block range of an L1 batch</td></tr><tr><td>zks_getBlockDetails</td><td>Extended L2 block details</td></tr><tr><td>zks_getTransactionDetails</td><td>Extended transaction details</td></tr><tr><td>zks_getRawBlockTransactions</td><td>Raw transactions in a block</td></tr><tr><td>zks_estimateFee</td><td>Estimate the full ZK fee</td></tr><tr><td>zks_estimateGasL1ToL2</td><td>Estimate gas for an L1-to-L2 transaction</td></tr><tr><td>zks_getAllAccountBalances</td><td>All token balances for an account</td></tr><tr><td>zks_getBridgeContracts</td><td>Bridge contract addresses</td></tr><tr><td>zks_getMainContract</td><td>Main (diamond) contract on L1</td></tr><tr><td>zks_getBridgehubContract</td><td>Bridgehub contract on L1</td></tr><tr><td>zks_getBaseTokenL1Address</td><td>Base token L1 address</td></tr><tr><td>zks_getL1GasPrice</td><td>Current L1 gas price</td></tr><tr><td>zks_getFeeParams</td><td>Current fee parameters</td></tr><tr><td>zks_getProtocolVersion</td><td>Current protocol version</td></tr><tr><td>zks_L1ChainId</td><td>L1 chain ID</td></tr><tr><td>zks_getL2ToL1LogProof</td><td>Proof of an L2-to-L1 message</td></tr></tbody></table>

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Lens Documentation](https://lens.xyz/)
* [ZKsync JSON-RPC Documentation](https://docs.zksync.io/)
* [Lens Explorer](https://explorer.lens.xyz/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
