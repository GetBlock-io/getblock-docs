# Hashkey

HashKey Chain is an EVM-compatible Layer-2 blockchain from HashKey Group, aimed at compliant, institution-grade on-chain finance. It runs a standard Ethereum execution layer, so Solidity contracts and the `eth_*` JSON-RPC surface, along with tooling such as Foundry, Hardhat, Ethers.js, and Viem, work unchanged. The native gas token is HSK. GetBlock serves HashKey Chain over three interfaces: JSON-RPC (HTTP), WebSocket, and an EIP-1767 GraphQL API.

## Key Features

* **EVM Compatible**: Runs standard Ethereum bytecode and tooling unchanged
* **HSK Gas**: Transaction fees are paid in HSK, the HashKey Platform Token (18 decimals)
* **Three Interfaces**: JSON-RPC over HTTP, WebSocket subscriptions, and an EIP-1767 GraphQL API
* **Full Node RPC**: The complete `eth_*` / `net_*` / `web3_*` surface plus `rpc_modules` and the `debug_*` trace namespace
* **EIP-1559 Fees**: Base-fee pricing with `eth_feeHistory` and priority fees, alongside legacy gas
* **GraphQL**: Request exactly the block, transaction, account, and log fields you need in one query

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. HashKey Chain implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and HashKey-specific behavior is documented at_ [_docs.hsk.xyz_](https://docs.hsk.xyz/)_._
{% endhint %}

## Network Information

| Property        | Value                        |
| --------------- | ---------------------------- |
| Network Name    | HashKey Chain Mainnet        |
| Chain ID        | 177 (0xb1)                   |
| Native Currency | HSK (HashKey Platform Token) |
| EVM Compatible  | Yes                          |
| Fee Model       | EIP-1559 + legacy            |

## Base URL

{% tabs %}
{% tab title="New York, USA" %}
```
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
HashKey Chain is served from the **New York** region only — use the New York base URL above. WebSocket is available at `wss://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/`, and the GraphQL API at the same base URL with `graphql` appended.
{% endhint %}

## Supported Networks

| Network | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Mainnet | 177      | ✅        | ✅   | ✅       | ❌                         | ❌                        | ❌                  | ✅             | ❌                    |

## Interfaces

HashKey Chain is served over three interfaces, each provisioned as its own endpoint in the GetBlock dashboard:

| Interface       | Protocol                             | Use it for                                                                                                                                    |
| --------------- | ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| JSON-RPC (HTTP) | Ethereum JSON-RPC 2.0 over HTTP      | Contract calls, transactions, and reads — the `eth_*` surface documented below                                                                |
| WebSocket       | Ethereum JSON-RPC 2.0 over WebSocket | `eth_subscribe` streams (newHeads, logs) and live data                                                                                        |
| GraphQL         | EIP-1767 GraphQL over HTTP           | Precise, nested reads of blocks, transactions, accounts, and logs — see [GraphQL API](/broken/pages/72d3fe92ba1e1850ebd36b27109d03a519274ad0) |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
## Setup project

```bash
mkdir hashkey-api-quickstart && cd hashkey-api-quickstart && npm init --yes
```
{% endstep %}

{% step %}
## Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
## Add code

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
## Run the script

```bash
node index.js
```
{% endstep %}

{% step %}
### Response

{% code overflow="wrap" %}
```
Latest Block: 29505683
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
## Setup project

```bash
mkdir hashkey-api-quickstart && cd hashkey-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endstep %}

{% step %}
## Install dependency

```bash
pip install requests
```
{% endstep %}

{% step %}
## Add code

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
## Run the script

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

| Method                   | Description                                   |
| ------------------------ | --------------------------------------------- |
| eth\_getBalance          | HSK balance of an address at a given block    |
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

## GraphQL API

In addition to JSON-RPC and WebSocket, HashKey Chain exposes an EIP-1767 GraphQL API at the `graphql` path, for precise, nested reads of blocks, transactions, accounts, and logs in a single query. See the [GraphQL API reference](graphql.md) for operations and example queries.

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [HashKey Chain Documentation](https://docs.hsk.xyz/)
* [GraphQL API](graphql.md)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [HashKey Explorer](https://hashkey.blockscout.com/)
