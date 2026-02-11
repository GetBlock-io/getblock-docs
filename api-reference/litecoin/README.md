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
https://go.getblock.io
```
{% endtab %}
{% endtabs %}

## Supported Networks

| Network | Chain ID | JSON RPC | Blockbook(WS) | Blockbook(REST) |
| ------- | -------- | -------- | ------------- | --------------- |
| Mainnet | 1329     | ✅        | ✅             | ✅               |

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

const url = `https://go.getblock.io/<ACCESS-TOKEN>/`;

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

{% tab title="undefined" %}
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
url = "https://go.getblock.io/<ACCESS-TOKEN>/"

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

### Blockchain Information

These methods allow users to retrieve information about the Litecoin blockchain state.

* `getblockchaininfo`: Returns an object containing various state info regarding blockchain processing.
* `getbestblockhash`: Returns the hash of the best (tip) block in the longest blockchain.
* `getblockcount`: Returns the number of blocks in the longest blockchain.
* `getdifficulty`: Returns the proof-of-work difficulty as a multiple of the minimum difficulty.

### Block Retrieval

Key methods to fetch block data from the blockchain.

* `getblock`: Returns an object with information about block for given hash.
* `getblockhash`: Returns hash of block in best-block-chain at height provided.
* `getblockstats`: Compute per block statistics for a given window.

### Transaction Methods

Methods for creating, decoding, and broadcasting transactions.

* `getrawtransaction`: Returns the raw transaction data.
* `decoderawtransaction`: Returns a JSON object representing the serialized transaction.
* `decodescript`: Decode a hex-encoded script.
* `createrawtransaction`: Creates a transaction spending the given inputs.

### Mempool Methods

Methods for querying the memory pool of unconfirmed transactions.

* `getmempoolinfo`: Returns details on the active state of the TX memory pool.
* `getmempoolentry`: Returns mempool data for given transaction.
* `getmempoolancestors`: Returns all in-mempool ancestors.
* `getmempooldescendants`: Returns all in-mempool descendants.

### Mining Methods

Methods related to mining operations and statistics.

* `getmininginfo`: Returns a json object containing mining-related information.
* `getnetworkhashps`: Returns the estimated network hashes per second.
* `getblocktemplate`: Returns data needed to construct a block to work on.

### Network Methods

Methods for querying network state and peer information.

* `getconnectioncount`: Returns the number of connections to other nodes.
* `getnettotals`: Returns information about network traffic.

### Utility Methods

General utility methods for validation and information.

* `validateaddress`: Return information about the given Litecoin address.
* `verifymessage`: Verify a signed message.
* `estimatesmartfee`: Estimates the approximate fee per kilobyte.
* `help`: List all commands, or get help for a specified command.

### UTXO Methods

Methods for querying unspent transaction outputs.

* `gettxout`: Returns details about an unspent transaction output.
* `gettxoutsetinfo`: Returns statistics about the unspent transaction output set.
* `gettxoutproof`: Returns a hex-encoded proof that txids were included in a block.

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
