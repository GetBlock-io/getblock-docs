---
description: >-
  Bitcoin Cash API Reference for fast and low-cost transactions with BCH nodes,
  offering scalable solutions for decentralized payments and peer-to-peer
  digital cash.
---

# Bitcoin Cash (BCH)

Bitcoin Cash is a Layer 1 proof-of-work blockchain that forked from Bitcoin in August 2017 to prioritize on-chain payment capacity. It raised the block size limit well beyond Bitcoin's, targeting low fees and fast confirmation for everyday transactions and peer-to-peer digital cash. The network is maintained by independent node implementations, including Bitcoin Cash Node, and retains the UTXO model and Bitcoin Core-style JSON-RPC interface.

### Key Features

* **Large Blocks**: A raised block size limit supports high on-chain transaction throughput
* **Low Fees**: Ample block space keeps per-transaction fees low under normal load
* **UTXO Model**: Uses the unspent transaction output model inherited from Bitcoin
* **CashAddr Addresses**: Uses the CashAddr format to reduce confusion with Bitcoin addresses
* **Proof of Work**: Secured by SHA-256 proof of work with a responsive difficulty adjustment
* **Block Finalization**: Bitcoin Cash Node finalizes deeply buried blocks against deep reorganizations
* **Schnorr Signatures**: Supports Schnorr signatures for smaller multisig and improved privacy
* **Bitcoin Core RPC**: Exposes a JSON-RPC interface familiar to Bitcoin developers

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Bitcoin Cash Node RPC methods is solely maintained and published through the official Bitcoin Cash Node documentation. This resource constitutes the sole authoritative reference implementation of the JSON-RPC 2.0 protocol interface across Bitcoin Cash node clients._
{% endhint %}

### Network Information

| Property        | Value                              |
| --------------- | ---------------------------------- |
| Network Name    | Bitcoin Cash Mainnet               |
| Native Currency | BCH (1 BCH = 100,000,000 satoshis) |
| Consensus       | Proof of Work (SHA-256)            |
| Address Format  | CashAddr and legacy Base58         |
| Model           | UTXO                               |
| Node Software   | Bitcoin Cash Node                  |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network | JSON RPC | REST | Blockbook (REST) | Blockbook (WebSocket) |
| ------- | -------- | ---- | ---------------- | --------------------- |
| Mainnet | ✅        | ✅    | <p></p><p>✅</p>  | ✅                     |

### Quickstart

In this section, you will learn how to make your first call with either:

* Javascript(Axios)
* Python

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir bch-api-quickstart
cd bch-api-quickstart
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

{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'getblockcount',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  const blockNumber = parseInt(response.data.result, 16);
  console.log('Current Block Number:', blockNumber);
})
.catch(error => console.error(error));
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
    "result": 961833,
    "error": null,
    "id": "getblock.io"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Set up the project directory

```bash
mkdir bch-api-quickstart
cd bch-api-quickstart
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
import json

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "getblockcount",
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

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

#### See Also

* [Bitcoin Cash Node Documentation](https://docs.bitcoincashnode.org/)
* [Bitcoin Cash Block Explorer](https://blockchair.com/bitcoin-cash)
* [Bitcoin Cash Developer Resources](https://bitcoincash.org/)
