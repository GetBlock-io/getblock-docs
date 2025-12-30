---
description: >-
  GetBlock provides fast and reliable access to Base nodes via JSON-RPC API.
  Connect to the Base network without running your own infrastructure.
---

# Base

### Overview

Base is a secure, low-cost, and developer-friendly Ethereum Layer 2 blockchain built by Coinbase using the OP Stack. It leverages optimistic rollup technology to deliver fast transactions at a fraction of Ethereum mainnet costs while inheriting Ethereum's security through data availability on L1.

### Key Features

* **Full EVM Compatibility**: Deploy Ethereum smart contracts without modification
* **OP Stack Architecture**: Built on Optimism's battle-tested Bedrock release
* **2-Second Block Time**: Fast block production for responsive applications
* **Low Transaction Fees**: Significantly cheaper than Ethereum mainnet
* **Ethereum Security**: Inherits security from Ethereum L1 through optimistic rollups
* **Coinbase Integration**: Seamless access to Coinbase ecosystem and fiat on-ramps
* **EIP-1559 Support**: Dynamic fee mechanism with base fee and priority fee
* **Fraud Proof Security**: Invalid state transitions can be challenged during dispute window

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

## Network Information

| Property          | Value                        |
| ----------------- | ---------------------------- |
| Network Name      | Base Mainnet                 |
| Chain ID          | 8453                         |
| Native Currency   | ETH                          |
| Block Time        | \~2 seconds                  |
| Gas Limit         | 25,000,000 per transaction   |
| EVM Compatibility | Full bytecode compatibility  |
| Consensus         | Optimistic Rollup (OP Stack) |
| Settlement Layer  | Ethereum L1                  |

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://go.getblock.io
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://go.getblock.us
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://go.getblock.asia
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON RPC | WSS | MEV Protected (JSON RPC) | MEV Protected (WSS) | Graph QL | New York, USA | Frankfurt, Germany | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------------------------ | ------------------- | -------- | ------------- | ------------------ | -------------------- |
| Mainnet | 8453     | ✅        | ✅   | ✅                        | ✅                   | ❌        | ✅             | ✅                  | ✅                    |
| Sepolia | 84532    | ✅        | ✅   | ❌                        | ❌                   | ✅        | ✅             | ❌                  | ❌                    |

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

### Quickstart with Axios

{% stepper %}
{% step %}
### Set up the project

Create a new project folder and initialize npm:

```bash
mkdir base-api-quickstart
cd base-api-quickstart
npm init --yes
```
{% endstep %}

{% step %}
### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
### Create the script file

Create a file named `index.js`. Also set the ES module type in your `package.json`:

Add `"type": "module"` to `package.json`.
{% endstep %}

{% step %}
### Add the request code

Put the following code into `index.js` (replace `<ACCESS-TOKEN>` with your GetBlock access token):

```javascript
import axios from 'axios';

const data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "eth_blockNumber",
    "params": [],
    "id": "getblock.io"
});

const config = {
    method: 'post',
    url: 'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers: {
        'Content-Type': 'application/json'
    },
    data: data
};

axios(config)
    .then(response => console.log(JSON.stringify(response.data)))
    .catch(error => console.log(error));
```
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

The block number will log in your console similar to:

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x2640163"
}
```
{% endstep %}
{% endstepper %}

### Quickstart with Python and Requests

{% stepper %}
{% step %}
### Set up the project folder

```bash
mkdir base-api-quickstart
cd base-api-quickstart
```
{% endstep %}

{% step %}
### Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate

# On Windows, use venv\Scripts\activate
```
{% endstep %}

{% step %}
### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
### Create the script

Create a file called `main.py` with the following code (replace `<ACCESS-TOKEN>` with your GetBlock access token):

```python
import requests
import json

url = "https://go.getblock.io/<ACCESS-TOKEN>/"

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
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}

## Available API Methods

GetBlock provides access to standard Ethereum JSON-RPC methods for the Base network.

### Reading Data (State Methods)

| Method                   | Description                                    |
| ------------------------ | ---------------------------------------------- |
| eth\_blockNumber         | Returns the current block number               |
| eth\_getBalance          | Returns the balance of an address              |
| eth\_getStorageAt        | Returns the value at a storage position        |
| eth\_getTransactionCount | Returns the transaction count (nonce)          |
| eth\_getCode             | Returns the code at an address                 |
| eth\_call                | Executes a call without creating a transaction |

### Block Information

| Method                                | Description                               |
| ------------------------------------- | ----------------------------------------- |
| eth\_getBlockByHash                   | Returns block information by hash         |
| eth\_getBlockByNumber                 | Returns block information by number       |
| eth\_getBlockReceipts                 | Returns all receipts for a block          |
| eth\_getBlockTransactionCountByHash   | Returns transaction count by block hash   |
| eth\_getBlockTransactionCountByNumber | Returns transaction count by block number |

### Transaction Methods

| Method                                   | Description                                   |
| ---------------------------------------- | --------------------------------------------- |
| eth\_getTransactionByHash                | Returns transaction by hash                   |
| eth\_getTransactionByBlockHashAndIndex   | Returns transaction by block hash and index   |
| eth\_getTransactionByBlockNumberAndIndex | Returns transaction by block number and index |
| eth\_getTransactionReceipt               | Returns the receipt of a transaction          |
| eth\_sendRawTransaction                  | Submits a signed transaction                  |

### Gas and Fee Estimation

| Method                    | Description                     |
| ------------------------- | ------------------------------- |
| eth\_gasPrice             | Returns the current gas price   |
| eth\_maxPriorityFeePerGas | Returns suggested priority fee  |
| eth\_feeHistory           | Returns fee history for blocks  |
| eth\_estimateGas          | Estimates gas for a transaction |

### Logs and Events

| Method       | Description                           |
| ------------ | ------------------------------------- |
| eth\_getLogs | Returns logs matching filter criteria |

### Chain Information

| Method              | Description            |
| ------------------- | ---------------------- |
| eth\_chainId        | Returns the chain ID   |
| eth\_syncing        | Returns sync status    |
| net\_version        | Returns the network ID |
| web3\_clientVersion | Returns client version |

### Debug Methods

| Method                    | Description                                |
| ------------------------- | ------------------------------------------ |
| debug\_traceTransaction   | Returns transaction trace                  |
| debug\_traceBlockByHash   | Traces all transactions in a block by hash |
| debug\_traceBlockByNumber | Traces block by number                     |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Base Developer Documentation](https://docs.base.org/)
* [Base Block Explorer - Basescan](https://basescan.org/)
* [Base Bridge](https://bridge.base.org/)
* [OP Stack Documentation](https://docs.optimism.io/)
* [Ethereum JSON-RPC Specification](https://ethereum.org/developers/docs/apis/json-rpc/)
