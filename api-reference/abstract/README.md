---
description: >-
  GetBlock provides fast and reliable access to Abstract nodes via JSON-RPC API.
  Connect to the Abstract network without running your own infrastructure.
---

# Abstract

Abstract is a zero-knowledge rollup Layer 2 on Ethereum, built with zkSync's ZK Stack and designed for consumer crypto applications. It is EVM-compatible — Solidity contracts and the Ethereum JSON-RPC interface work as expected — while running on the ZK Stack's EraVM, which brings native account abstraction (every account can be a smart account, powering the Abstract Global Wallet, paymasters, gas sponsorship, and social logins). Batches of L2 transactions are proven with validity (ZK) proofs and settled on Ethereum, which also provides data availability. Gas is paid in ETH. Beyond the standard `eth_*` methods, Abstract exposes the ZK Stack's `zks_*` methods for L2-specific features such as batch details, fee estimation, and L2-to-L1 proofs.

### Key Features

* **EVM Compatibility**: Solidity contracts and the Ethereum JSON-RPC interface work as expected (contracts compile to EraVM via zksolc)
* **Native Account Abstraction**: Every account can be a smart account, powering the Abstract Global Wallet, paymasters, and gas sponsorship
* **Validity (ZK) Rollup**: Transactions are proven with zero-knowledge validity proofs and settled on Ethereum L1
* **ETH Gas**: Fees are paid in ETH, with a ZK fee model that prices L2 execution and L1 data availability
* **zks Extensions**: ZK Stack `zks_*` methods expose batches, fee parameters, bridge contracts, and L2-to-L1 proofs
* **Elastic Network**: Part of the ZK Stack Elastic Network, sharing standards with other ZK chains

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Abstract implements the standard Ethereum JSON-RPC interface plus the zkSync ZK Stack `zks_*` methods; the canonical specifications are the Ethereum JSON-RPC specification at_ [_ethereum.org_](https://ethereum.org/en/developers/docs/apis/json-rpc/) _and the ZKsync API reference. Abstract-specific behaviour is documented at_ [_docs.abs.xyz_](https://docs.abs.xyz/)_._
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Abstract</td></tr><tr><td>Chain ID</td><td>2741</td></tr><tr><td>Native Currency</td><td>ETH</td></tr><tr><td>EVM Compatible</td><td>Yes (zkEVM / EraVM via zksolc)</td></tr><tr><td>Stack</td><td>zkSync ZK Stack</td></tr><tr><td>Settlement &#x26; DA</td><td>Ethereum L1</td></tr><tr><td>Block Time</td><td>~1-2 seconds</td></tr><tr><td>Finality</td><td>L1-backed after ZK proof settlement</td></tr><tr><td>Account Model</td><td>Native account abstraction</td></tr></tbody></table>

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
| Mainnet | 2741     | ✅        | ✅   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir abstract-api-quickstart && cd abstract-api-quickstart && npm init --yes
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
Latest Block: 0x501390a
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
mkdir abstract-api-quickstart && cd abstract-api-quickstart
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
| eth\_getBalance          | ETH balance of an address at a given block |
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

| Method                       | Description                              |
| ---------------------------- | ---------------------------------------- |
| zks\_L1BatchNumber           | Latest L1 batch number                   |
| zks\_getL1BatchDetails       | Details of an L1 batch                   |
| zks\_getL1BatchBlockRange    | L2 block range of an L1 batch            |
| zks\_getBlockDetails         | Extended L2 block details                |
| zks\_getTransactionDetails   | Extended transaction details             |
| zks\_getRawBlockTransactions | Raw transactions in a block              |
| zks\_estimateFee             | Estimate the full ZK fee                 |
| zks\_estimateGasL1ToL2       | Estimate gas for an L1-to-L2 transaction |
| zks\_getBridgeContracts      | Bridge contract addresses                |
| zks\_getMainContract         | Main (diamond) contract on L1            |
| zks\_getBridgehubContract    | Bridgehub contract on L1                 |
| zks\_getBaseTokenL1Address   | Base token L1 address                    |
| zks\_getL1GasPrice           | Current L1 gas price                     |
| zks\_getFeeParams            | Current fee parameters                   |
| zks\_getProtocolVersion      | Current protocol version                 |
| zks\_L1ChainId               | L1 chain ID                              |
| zks\_getL2ToL1LogProof       | Proof of an L2-to-L1 message             |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Abstract Documentation](https://docs.abs.xyz/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/en/developers/docs/apis/json-rpc/)
* [ZKsync JSON-RPC API Reference](https://docs.zksync.io/zksync-protocol/api/zks-rpc)
* [Abscan Explorer](https://abscan.org/)
* [Abstract Website](https://abs.xyz/)
