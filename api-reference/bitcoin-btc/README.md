---
description: >-
  Bitcoin Network API Reference for secure and scalable interaction with BTC
  nodes, enabling peer-to-peer transactions and decentralized finance on the
  world’s first cryptocurrency block
---

# Bitcoin (BTC)

Bitcoin is the first decentralized cryptocurrency, launched in 2009 by the pseudonymous Satoshi Nakamoto as a peer-to-peer electronic cash system. It is a Layer 1 proof-of-work blockchain secured by SHA-256 mining, with a fixed supply of 21 million BTC and an average block time of ten minutes. The network is maintained by independent node implementations, principally Bitcoin Core, and uses the UTXO model with a widely adopted JSON-RPC interface.

### Key Features

* **UTXO Model**: Balances are derived from unspent transaction outputs rather than account states
* **Proof of Work**: Secured by SHA-256 mining with a difficulty adjustment every 2016 blocks
* **21 Million Supply Cap**: A fixed maximum supply enforced by consensus
* **\~10-Minute Block Time**: Blocks are found roughly every ten minutes on average
* **SegWit and Taproot**: Modern upgrades enabling witness data, lower fees, and Schnorr signatures
* **PSBT Support**: Partially Signed Bitcoin Transactions for multi-party and hardware-wallet signing
* **Address Formats**: Legacy Base58 (P2PKH, P2SH), native SegWit (bech32), and Taproot (bech32m)
* **Bitcoin Core RPC**: Exposes a JSON-RPC interface familiar to Bitcoin developers

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification of Bitcoin Core RPC methods is maintained and published solely in the official Bitcoin Core documentation. This resource is the sole authoritative reference implementation of the JSON-RPC 2.0 protocol for Bitcoin node clients._
{% endhint %}

### Network Information

| Property        | Value                                             |
| --------------- | ------------------------------------------------- |
| Network Name    | Bitcoin Mainnet                                   |
| Native Currency | BTC (1 BTC = 100,000,000 satoshis)                |
| Consensus       | Proof of Work (SHA-256)                           |
| Address Format  | bech32m (Taproot), bech32 (SegWit), legacy Base58 |
| Model           | UTXO                                              |
| Node Software   | Bitcoin Core                                      |

### Base URL

```bash
https://shared.eu-central-1.getblock.io
```

### Supported Networks

| Network | JSON RPC | REST | Blockbook (REST) | Blockbook (WebSocket) |
| ------- | -------- | ---- | ---------------- | --------------------- |
| Mainnet | ✅        | ✅    | ✅                | ✅                     |
| Testnet | ✅        | ✅    | ✅                | ❌                     |

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
mkdir btc-api-quickstart
cd btc-api-quickstart
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
  console.log('Current Block Number:', response.data.result);
})
.catch(error => console.error(error));
```
{% endcode %}

Replace `<ACCESS-TOKEN>` with your actual GetBlock access token.
{% endstep %}

{% step %}
### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "result": 830000,
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
mkdir btc-api-quickstart
cd btc-api-quickstart
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

* [Bitcoin Core Documentation](https://developer.bitcoin.org/reference/rpc/)
* [Bitcoin Block Explorer](https://mempool.space/)
* [Bitcoin Developer Resources](https://bitcoin.org/en/development)
