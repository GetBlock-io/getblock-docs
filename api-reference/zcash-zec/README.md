---
description: >-
  GetBlock provides fast and reliable access to Zcash nodes via JSON-RPC API.
  Connect to the Zcash network without running your own infrastructure.
---

# Zcash(ZEC)

Zcash is a proof-of-work privacy-preserving cryptocurrency built by the Electric Coin Company and the Zcash Foundation, using zk-SNARKs to enable optional shielded transactions with unlinkable senders, recipients, and amounts. The network exposes a Bitcoin Core-compatible JSON-RPC interface extended with `z_*` methods for shielded-pool operations. GetBlock serves Zcash from `zebrad`, the Rust full node maintained by the Zcash Foundation, which has replaced the deprecated `zcashd`. Zebra's JSON-RPC interface follows Bitcoin Core's method names but returns a smaller set of fields for several of them, so clients ported from Bitcoin Core should check each method's response rather than assume parity.

### Key Features

* **Optional Shielded Transactions**: Sapling and Orchard pools provide zk-SNARK-backed transaction privacy alongside transparent Bitcoin-style UTXOs
* **NU6.3 Consensus**: Latest Zcash network upgrade with Ironwood note commitment tree extension for improved shielded-pool queries
* **Bitcoin Core-Style RPC**: Familiar interface for infrastructure developers — same method names, response formats, and authentication as `bitcoind`
* **Unified Addresses**: Standardized addresses combining transparent, Sapling, and Orchard receivers into a single string
* **Equihash Proof-of-Work**: Memory-hard mining algorithm resistant to specialized ASIC dominance

{% hint style="info" %}
_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Zcash JSON-RPC methods is solely maintained and published through the official Zcash Foundation documentation portal at_ [_zebra.zfnd.org_](https://zebra.zfnd.org) _and the zebra-rpc crate at_ [_docs.rs_](https://docs.rs)
{% endhint %}

### Network Information

| Property        | Value              |
| --------------- | ------------------ |
| Network Name    | Zcash              |
| Native Currency | ZEC                |
| Decimals        | 8                  |
| Block Time      | 75 seconds         |
| Consensus       | Proof-of-Work(PoW) |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network | JSON-RPC | Blockbook (REST) | Blockbook (WebSocket) |
| ------- | -------- | ---------------- | --------------------- |
| Mainnet | ✅        | ✅                | ✅                     |

Mainnet is served from the Frankfurt, Germany region.

### Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

Create and initialize a new project:

```bash
mkdir zec-api-quickstart
cd zec-api-quickstart
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
  method: 'getinfo',
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

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "version": 6000000,
        "build": "v6.0.0",
        "subversion": "/Zebra:6.0.0/",
        "protocolversion": 170160,
        "blocks": 3420502,
        "connections": 41,
        "difficulty": 229698549.25032642,
        "testnet": false,
        "paytxfee": 0.0,
        "relayfee": 1e-6,
        "errors": "chain updates have stalled, state height has not increased for 10 minutes. Hint: check your network connection, and your computer clock and time zone",
        "errorstimestamp": 1784662715
    }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
### Set up the project directory

```bash
mkdir zec-api-quickstart
cd zec-api-quickstart
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
    "method": "getinfo",
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

### Available Methods

Zcash is documented across three interfaces, each provisioned as its own endpoint.

#### [Zcash JSON-RPC API](zcash-json-rpc-api/)

The Zebra node interface: blocks, transactions, mempool, mining, transparent address queries, and `z_*` methods for the shielded pools.

#### Zcash Blockbook REST API

Address- and xpub-indexed queries over HTTP for transparent addresses: balances, transaction history, unspent outputs, balance history, and fiat rates.

#### Zcash Blockbook (WebSocket) API

The same indexed queries over a persistent connection, plus subscriptions to new blocks and to activity on a set of addresses.

{% hint style="info" %}
Every interface sees **transparent** (`t1`, `t3`) activity only. Shielded Sapling and Orchard notes are encrypted to the holder's viewing key, so no public endpoint can list their balances or history.
{% endhint %}

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)
