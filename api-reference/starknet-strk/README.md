# StarkNet (STRK)

Starknet is a permissionless validity (ZK) rollup that settles to Ethereum, executing contracts on the Cairo VM and proving that execution with STARK proofs verified on Ethereum L1. It is not EVM-compatible: contracts are Cairo classes, values are felt252 field elements expressed as hex, and every account is itself a contract under Starknet's native account abstraction. Blocks are addressed by a `block_id` that is either an object (a block number or block hash) or a tag (`latest`, `pending`). The JSON-RPC surface is the standard `starknet_*` method set: block and state reads, transaction and receipt lookups, contract calls and class/storage reads, event queries, fee estimation, and the invoke/declare/deploy-account write methods.

### Key Features

* **Validity Rollup**: Execution is proven with STARK proofs and verified on Ethereum L1, inheriting L1 security
* **Cairo VM**: Contracts are Cairo classes compiled to Sierra, not EVM bytecode; values are felt252 field elements
* **Native Account Abstraction**: Every account is a contract; signature schemes and validation are programmable
* **block\_id Addressing**: Reads accept a block number, block hash, or the `latest` / `pending` tags
* **Dual Fee Tokens**: Fees are payable in ETH or STRK; fee amounts carry a unit (`WEI` or `FRI`)
* **Two-Stage Finality**: Transactions move from `ACCEPTED_ON_L2` to `ACCEPTED_ON_L1` once settled on Ethereum
* **Classes and Instances**: A class is declared once by hash, then deployed to many contract addresses

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for the Starknet JSON-RPC interface is maintained and published by StarkWare and the Starknet community through the official documentation at_ [_docs.starknet.io_](https://docs.starknet.io/) _and the OpenRPC specification in the_ [_starknet-specs_](https://github.com/starkware-libs/starknet-specs) _repository. This resource constitutes the sole authoritative reference for the starknet\__ JSON-RPC methods.\*
{% endhint %}

## Network Information

<table data-search="false"><thead><tr><th>Property</th><th>Value</th></tr></thead><tbody><tr><td>Network Name</td><td>Starknet</td></tr><tr><td>Chain ID</td><td>0x534e5f4d41494e (SN_MAIN)</td></tr><tr><td>Native Fee Tokens</td><td>ETH, STRK</td></tr><tr><td>Decimals</td><td>18</td></tr><tr><td>Virtual Machine</td><td>Cairo VM (felt252)</td></tr><tr><td>Settlement Layer</td><td>Ethereum L1 (STARK validity rollup)</td></tr><tr><td>Account Model</td><td>Native account abstraction</td></tr><tr><td>Finality</td><td>L1-backed after proof settlement</td></tr></tbody></table>

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
| Mainnet | SN\_MAIN | ✅        | ❌   | ❌       | ❌                         | ❌                        | ✅                  | ❌             | ❌                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

{% code overflow="wrap" %}
```bash
mkdir starknet-api-quickstart && cd starknet-api-quickstart && npm init --yes
```
{% endcode %}
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Add code

{% code title="index.js" overflow="wrap" %}
```javascript
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'starknet_blockNumber',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  console.log('Latest block:', response.data.result);
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
    "result": 14293145,
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
mkdir starknet-api-quickstart && cd starknet-api-quickstart
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
    "method": "starknet_blockNumber",
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

| Method                       | Description                                  |
| ---------------------------- | -------------------------------------------- |
| starknet\_specVersion        | Version of the Starknet JSON-RPC spec served |
| starknet\_chainId            | Chain ID of the connected Starknet network   |
| starknet\_syncing            | Sync status, or false when fully synced      |
| starknet\_blockNumber        | Latest accepted block number                 |
| starknet\_blockHashAndNumber | Latest block hash and number together        |

### Blocks & State

| Method                             | Description                                |
| ---------------------------------- | ------------------------------------------ |
| starknet\_getBlockWithTxHashes     | Block header plus transaction hashes       |
| starknet\_getBlockWithTxs          | Block header plus full transaction bodies  |
| starknet\_getBlockWithReceipts     | Block with transactions and their receipts |
| starknet\_getBlockTransactionCount | Number of transactions in a block          |
| starknet\_getStateUpdate           | State diff applied by a block              |

### Transactions

| Method                                    | Description                                    |
| ----------------------------------------- | ---------------------------------------------- |
| starknet\_getTransactionByHash            | Transaction body by hash                       |
| starknet\_getTransactionByBlockIdAndIndex | Transaction by block and index                 |
| starknet\_getTransactionStatus            | Finality and execution status of a transaction |
| starknet\_getTransactionReceipt           | Execution receipt of a transaction             |

### Contracts, Accounts & State

<table data-search="false"><thead><tr><th>Method</th><th>Description</th></tr></thead><tbody><tr><td>starknet_call</td><td>Call a contract view function (no transaction)</td></tr><tr><td>starknet_getStorageAt</td><td>Raw storage value at a contract key</td></tr><tr><td>starknet_getNonce</td><td>Account nonce at a block</td></tr><tr><td>starknet_getClassHashAt</td><td>Class hash deployed at an address</td></tr><tr><td>starknet_getClass</td><td>Contract class definition by class hash</td></tr><tr><td>starknet_getClassAt</td><td>Contract class deployed at an address</td></tr><tr><td>starknet_getEvents</td><td>Filtered events with pagination</td></tr></tbody></table>

### Fee Estimation

| Method                       | Description                              |
| ---------------------------- | ---------------------------------------- |
| starknet\_estimateFee        | Estimate the fee for transactions        |
| starknet\_estimateMessageFee | Estimate the fee for an L1-to-L2 message |

### Write API

| Method                                | Description                        |
| ------------------------------------- | ---------------------------------- |
| starknet\_addInvokeTransaction        | Submit a signed invoke transaction |
| starknet\_addDeployAccountTransaction | Deploy a new account contract      |
| starknet\_addDeclareTransaction       | Declare a new contract class       |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Starknet Documentation](https://docs.starknet.io/)
* [Starknet JSON-RPC Spec (starknet-specs)](https://github.com/starkware-libs/starknet-specs)
* [Starknet Book](https://book.starknet.io/)
* [Starkscan Explorer](https://starkscan.co/)
* [Voyager Explorer](https://voyager.online/)
