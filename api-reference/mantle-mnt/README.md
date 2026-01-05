---
description: >-
  GetBlock provides fast and reliable access to Mantle nodes via JSON-RPC API.
  Connect to the Mantle network without running your own infrastructure.
---

# Mantle (MNT)

Mantle is a high-performance Ethereum Layer 2 scaling solution built with a modular architecture. It combines an optimistic rollup protocol with EigenDA for data availability, delivering low fees and high security while maintaining full EVM compatibility. Mantle separates core blockchain functions into specialized modules for optimal performance.

### Key Features

* **Full EVM Compatibility**: Deploy Ethereum smart contracts without modification
* **Modular Architecture**: Separates execution, data availability, and settlement layers
* **EigenDA Integration**: First L2 to use EigenDA for efficient data availability
* **MNT Native Token**: Uses MNT for gas fees and governance
* **Low Transaction Fees**: Significantly cheaper than Ethereum mainnet
* **Ethereum Security**: Inherits security from Ethereum L1 through optimistic rollups
* **7-Day Dispute Window**: Fraud proof period for transaction finality
* **High Throughput**: Designed for 500+ TPS with modular scaling

{% hint style="info" %}
TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION.

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Ethereum Virtual Machine (EVM) JSON-RPC methods is solely maintained and published through the official Ethereum Foundation documentation portal at_ [_ethereum.org_](http://ethereum.org/)_. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across EVM-compatible execution clients._
{% endhint %}

## Network Information

| Property          | Value                       |
| ----------------- | --------------------------- |
| Network Name      | Mantle Mainnet              |
| Chain ID          | 5000                        |
| Native Currency   | MNT                         |
| RPC URL           | https://rpc.mantle.xyz      |
| EVM Compatibility | Full bytecode compatibility |
| Consensus         | Optimistic Rollup (Modular) |
| Data Availability | EigenDA                     |
| Settlement Layer  | Ethereum L1                 |

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

| Network | Chain ID | JSON RPC | WSS | New York, USA | Frankfurt, Germany | Singapore, Singapore |
| ------- | -------- | -------- | --- | ------------- | ------------------ | -------------------- |
| Mainnet | 5000     | ✅        | ✅   | ✅             | ✅                  | ✅                    |

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

{% hint style="warning" %}
Before you begin, you must have already installed `npm` or `yarn` on your local machine (for the Axios example) or Python and pip (for the Python example).&#x20;

Refer to:

* [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
* [yarn](https://classic.yarnpkg.com/lang/en/docs/install)
{% endhint %}

### Quickstart with Axios

{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir mantle-api-quickstart
cd mantle-api-quickstart
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
### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
### Add code

Add the following code to `index.js`:

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

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": "0x3f67f8"
}
```
{% endstep %}
{% endstepper %}

### Quickstart with Python and Requests

{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir mantle-api-quickstart
cd mantle-api-quickstart
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
### Create script

Create a file called `main.py` with the following content:

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

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
{% endstep %}

{% step %}
### Run the script

```bash
python main.py
```
{% endstep %}
{% endstepper %}

## Available API Methods

GetBlock provides access to standard Ethereum JSON-RPC methods for the Mantle network.

#### Transaction Methods

| Method                                   | Description                                   |
| ---------------------------------------- | --------------------------------------------- |
| eth\_getTransactionByHash                | Returns transaction by hash                   |
| eth\_getTransactionByBlockHashAndIndex   | Returns transaction by block hash and index   |
| eth\_getTransactionByBlockNumberAndIndex | Returns transaction by block number and index |
| eth\_getTransactionCount                 | Returns the transaction count (nonce)         |
| eth\_getTransactionReceipt               | Returns the receipt of a transaction          |
| eth\_sendRawTransaction                  | Submits a signed transaction                  |

#### Account/State Methods

| Method            | Description                                    |
| ----------------- | ---------------------------------------------- |
| eth\_getBalance   | Returns the balance of an address              |
| eth\_getStorageAt | Returns the value at a storage position        |
| eth\_getCode      | Returns the code at an address                 |
| eth\_call         | Executes a call without creating a transaction |
| eth\_getProof     | Returns account and storage proof              |
| eth\_accounts     | Returns a list of addresses owned by client    |

#### Gas and Fee Methods

| Method                    | Description                        |
| ------------------------- | ---------------------------------- |
| eth\_gasPrice             | Returns the current gas price      |
| eth\_estimateGas          | Estimates gas for a transaction    |
| eth\_feeHistory           | Returns historical gas information |
| eth\_maxPriorityFeePerGas | Returns current max priority fee   |

#### Filter Methods

| Method                | Description                            |
| --------------------- | -------------------------------------- |
| eth\_getLogs          | Returns logs matching filter criteria  |
| eth\_newFilter        | Creates a log filter                   |
| eth\_newBlockFilter   | Creates a block filter                 |
| eth\_getFilterChanges | Returns filter changes since last poll |
| eth\_getFilterLogs    | Returns all logs matching filter       |
| eth\_uninstallFilter  | Removes a filter                       |

#### Subscription Methods (WebSocket)

| Method           | Description                       |
| ---------------- | --------------------------------- |
| eth\_subscribe   | Creates a subscription for events |
| eth\_unsubscribe | Cancels an existing subscription  |

#### Network/Chain Methods

| Method              | Description                       |
| ------------------- | --------------------------------- |
| eth\_chainId        | Returns the chain ID              |
| eth\_syncing        | Returns sync status               |
| eth\_mining         | Returns mining status             |
| net\_version        | Returns the network ID            |
| net\_listening      | Returns listening status          |
| net\_peerCount      | Returns number of connected peers |
| web3\_clientVersion | Returns client version            |
| web3\_sha3          | Returns Keccak-256 hash of data   |

#### Debug Methods

| Method                    | Description                                |
| ------------------------- | ------------------------------------------ |
| debug\_traceBlockByHash   | Traces all transactions in a block by hash |
| debug\_traceBlockByNumber | Traces block by number                     |
| debug\_traceTransaction   | Traces a specific transaction              |
| debug\_traceCall          | Traces a call without creating transaction |

## Support

For technical support and questions:

* Support: support@getblock.io

## See Also

* [Mantle Developer Documentation](https://docs.mantle.xyz/)
* [Mantle Block Explorer - Mantlescan](https://mantlescan.xyz/)
* [Mantle Bridge](https://bridge.mantle.xyz/)
* [Mantle SDK](https://github.com/mantlenetworkio/mantle)
* [Ethereum JSON-RPC Specification](https://ethereum.org/developers/docs/apis/json-rpc/)
