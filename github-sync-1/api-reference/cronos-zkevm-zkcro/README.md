---
description: >-
  GetBlock provides fast and reliable access to Cronos zkEVM nodes via JSON-RPC
  API. Connect to the Cronos zkEVM network without running your own
  infrastructure.
---

# Cronos zkEVM (zkCRO)

Cronos zkEVM is a zero-knowledge Ethereum Layer-2 rollup built on the ZKsync ZK Stack (the ZKsync Elastic Chain).&#x20;

It is EVM-compatible at the application level, so Solidity contracts run with the ZKsync toolchain (`zksolc`), and the Ethereum-style `eth_*` JSON-RPC surface is available for reads, transactions, and subscriptions.&#x20;

The native gas token is zkCRO. This reference document describes the Ethereum-compatible JSON-RPC interface, available over HTTP and WebSocket.

### Key Features

* **ZKsync ZK Stack**: A zero-knowledge rollup on the ZKsync Elastic Chain, settling to Ethereum with validity proofs
* **EVM Compatible**: Solidity contracts run after compilation with the ZKsync compiler (`zksolc`)
* **zkCRO Gas**: Transaction fees are paid in zkCRO
* **ZK Validity Proofs**: Transactions are finalized on Ethereum through zero-knowledge proofs
* _eth\_ JSON-RPC_\*: The widely-supported Ethereum method surface over HTTP and WebSocket
* **zks\_ Namespace**: ZKsync-stack nodes also expose chain-specific `zks_*` methods (batch and L1/L2 bridging data) — see the note below

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Cronos zkEVM implements the standard Ethereum JSON-RPC interface; the canonical specification for these methods is the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/)_, and Cronos zkEVM-specific behaviour is documented at_ [_Cronos zkEVM Documentation_](https://docs-zkevm.cronos.org/)_. Cronos zkEVM is a ZKsync ZK Stack chain; it also exposes a chain-specific `zks_*` namespace not covered here, and contracts require the `zksolc` compiler._
{% endhint %}

## Network Information

| Property         | Value                         |
| ---------------- | ----------------------------- |
| Network Name     | Cronos zkEVM Mainnet          |
| Chain ID         | 388                           |
| Native Currency  | zkCRO                         |
| EVM Compatible   | Yes (ZKsync ZK Stack, zksolc) |
| Settlement Layer | Ethereum                      |
| Fee Model        | EIP-1559 + legacy             |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Cronos zkEVM is served from the **Frankfurt** region only — use the Frankfurt base URL above. WebSocket is available at `wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`.
{% endhint %}

## Supported Networks

| Network              | Chain ID | JSON-RPC | WSS | GraphQL | MEV protected (WebSocket) | MEV protected (JSON-RPC) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| -------------------- | -------- | -------- | --- | ------- | ------------------------- | ------------------------ | ------------------ | ------------- | -------------------- |
| Cronos zkEVM Mainnet | 388      | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir cronos-zkevm-api-quickstart && cd cronos-zkevm-api-quickstart && npm init --yes
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
Latest block: 2679088
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
mkdir cronos-zkevm-api-quickstart && cd cronos-zkevm-api-quickstart
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

| Method                   | Description                                  |
| ------------------------ | -------------------------------------------- |
| eth\_getBalance          | zkCRO balance of an address at a given block |
| eth\_accounts            | Accounts owned by the client                 |
| eth\_getStorageAt        | Value at a specific contract storage slot    |
| eth\_getTransactionCount | Address nonce (transaction count)            |
| eth\_getCode             | Deployed bytecode at an address              |

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

* [Cronos zkEVM Documentation](https://docs-zkevm.cronos.org/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [Cronos zkEVM Explorer](https://explorer.zkevm.cronos.org/)
