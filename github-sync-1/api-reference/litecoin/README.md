---
description: >-
  GetBlock provides RPC endpoints that implement the Litecoin JSON-RPC API
  standard. These methods are core functionalities for interacting with Litecoin
  nodes.
---

# Litecoin

The Litecoin network offers a comprehensive suite of methods that enable developers to interact seamlessly with its blockchain infrastructure. This overview provides an in-depth examination of these methods, categorizing them into key functional areas for better understanding and implementation.

What you can expect to find in this documentation

* Compatibility with the Litecoin mainnet and testnets
* Purpose, functionality, and use cases of each Litecoin method
* Required input parameters
* Sample requests and responses
* Code examples in multiple programming languages (Python, JavaScript)

What you can do with this API:

* Query real-time blockchain data
* Retrieve block and transaction information
* Monitor mempool and network status
* Submit and track transactions
* Access mining statistics and difficulty

{% hint style="info" %}
_**TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION.**_

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and streamlined developer experience optimization. The canonical and normative specification for Litecoin JSON-RPC methods is solely maintained and published through the official Litecoin Core documentation._
{% endhint %}

## Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | JSON RPC | Blockbook (REST) | Blockbook (WebSocket) |
| ------- | -------- | ---------------- | --------------------- |
| Mainnet | ✅        | ✅                | ✅                     |

{% hint style="info" %}
JSON-RPC is the Litecoin Core node interface. Blockbook is a separate add-on providing an address- and xpub-indexed view of the chain, and its REST and WebSocket interfaces are provisioned as their own endpoints with their own URLs. A JSON-RPC endpoint does not answer address, UTXO, or wallet queries, and the Litecoin Core wallet RPCs such as `listunspent` are disabled on shared nodes. See the [Blockbook add-on](../../add-ons/blockbook.md).
{% endhint %}

| Interface                                                | Use it for                                                                          |
| -------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| [JSON-RPC](litecoin-json-rpc-api/)                       | Node-level queries: blocks, raw transactions, mempool, mining, network state        |
| [Blockbook REST](litecoin-blockbook-rest-api/)           | Address balances, UTXOs, wallet-level xpub queries, transaction history, fiat rates |
| [Blockbook WebSocket](litecoin-blockbook-websocket-api/) | The same queries plus live subscriptions to new blocks and address activity         |

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

Before you begin, you must have already installed [`npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) or [`yarn`](https://classic.yarnpkg.com/lang/en/docs/install) on your local machine (for the Axios example) or Python and pip (for the Python example).

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

{% tabs %}
{% tab title="npm" %}
```bash
mkdir litecoin-api-quickstart
cd litecoin-api-quickstart
npm init --yes
```
{% endtab %}

{% tab title="yarn" %}
```bash
mkdir litecoin-api-quickstart
cd litecoin-api-quickstart
yarn init -y
```
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### Install Axios

{% tabs %}
{% tab title="npm" %}
```bash
npm install axios
```
{% endtab %}

{% tab title="yarn" %}
```bash
yarn add axios
```
{% endtab %}
{% endtabs %}
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

{% code title="index.js" %}
```javascript
import axios from "axios";

const url = `https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/`;

const payload = {
  jsonrpc: '2.0',
  id: 1,
  method: 'getblockcount',
  params: []
};

axios.post(url, payload)
  .then(response => {
    console.log('Current block height:', response.data.result);
  })
  .catch(error => {
    console.error(error);
  });
```
{% endcode %}

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
    "result": 3050080,
    "error": null,
    "id": "getblock.io"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab %}
{% stepper %}
{% step %}
### Setup the project directory

```bash
mkdir litecoin-api-quickstart
cd litecoin-api-quickstart
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

{% code title="main.py" %}
```python
import requests

# Replace <ACCESS-TOKEN> with your actual API key from GetBlock
url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

# Create a JSON-RPC payload
payload = {
    "jsonrpc": "2.0",
    "id": 1,
    "method": "getblockcount",
    "params": []
}

headers = {
    "Content-Type": "application/json"
}

try:
    # Send the POST request
    response = requests.post(url, json=payload, headers=headers)
    response.raise_for_status()  # Check for HTTP errors
    data = response.json()

    # Print the result
    print(f"Current block height: {data['result']}")
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")
except KeyError:
    print("Unexpected response format:", response.text)
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual access token from GetBlock.
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

## Available Methods

Litecoin is documented across three interfaces, each provisioned as its own endpoint.

1. [Litecoin JSON-RPC API](litecoin-json-rpc-api/): The Litecoin Core node interface: blocks, raw transactions, mempool inspection, mining data, network state, and fee estimation. Methods are grouped by area on the section page.
2. [Litecoin Blockbook REST API](litecoin-blockbook-rest-api/): Address- and xpub-indexed queries over HTTP: balances, transaction history, unspent outputs, wallet-level xpub lookups, balance history, and fiat rates.
3. [Litecoin Blockbook (WebSocket) API](litecoin-blockbook-websocket-api/): The same indexed queries over a persistent connection, plus subscriptions to new blocks and to activity on a set of addresses.

{% hint style="info" %}
A JSON-RPC endpoint has no address index, so address balances and UTXO lookups belong on Blockbook. The Litecoin Core wallet RPCs, including `listunspent`, are disabled on shared nodes because those nodes carry no user wallets.
{% endhint %}

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Litecoin Developer Documentation](https://litecoin.info/docs)
* [Litecoin Core GitHub](https://github.com/litecoin-project/litecoin)
* [Litecoin Block Explorer (Blockchair)](https://blockchair.com/litecoin)
* [Litecoin Block Explorer (SoChain)](https://sochain.com/LTC)
* [Litecoin Block Explorer (Litecoinspace)](https://litecoinspace.org/)
* [Bitcoin JSON-RPC API](https://developer.bitcoin.org/reference/rpc/)
* [Litecoin Foundation Website](https://litecoin.org/)
* [MWEB Documentation](https://github.com/litecoin-project/lips/blob/master/lip-0002.mediawiki)
