---
description: >-
  GetBlock provides fast and reliable access to Zcash nodes via JSON-RPC API.
  Connect to the Zcash network without running your own infrastructure.
---

# Zcash(ZEC)

Zcash is a proof-of-work privacy-preserving cryptocurrency built by the Electric Coin Company and the Zcash Foundation, using zk-SNARKs to enable optional shielded transactions with unlinkable senders, recipients, and amounts. The network exposes a Bitcoin Core-compatible JSON-RPC interface extended with `z_*` methods for shielded-pool operations. As of the Zebra 3.0.0 release (January 2026), the reference node implementation is `zebrad` — a Rust-based full node replacing the deprecated `zcashd` (end-of-life July 18, 2026) — with full support for NU6.3, the Ironwood note commitment tree extension, and three new RPCs (`getnetworkinfo`, `getmempoolinfo`, side chain queries).

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

| Network | JSON-RPC | Blockbook(REST) | Frankfurt, Germany |
| ------- | -------- | --------------- | ------------------ |
| Mainnet | ✅        | ✅               | ✅                  |

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

#### Node & Client Info

| Method           | Description                                               |
| ---------------- | --------------------------------------------------------- |
| `getinfo`        | Basic node status, version, block height, connections     |
| `getnetworkinfo` | Network status, peer info, protocol version (Zebra 3.0.0) |

#### Blockchain State

| Method                      | Description                                               |
| --------------------------- | --------------------------------------------------------- |
| `getblockchaininfo`         | Chain state, activated upgrades, value pool balances      |
| `getbestblockhash`          | Hash of the chain tip                                     |
| `getbestblockheightandhash` | Chain tip height and hash together (Zebra-specific)       |
| `getblockcount`             | Chain tip height                                          |
| `getblockhash`              | Block hash by height                                      |
| `getblock`                  | Block by hash or height (verbosity 0/1/2, includes `nTx`) |
| `getblockheader`            | Block header only                                         |
| `getdifficulty`             | Proof-of-work difficulty multiplier                       |

#### Transactions

| Method               | Description                             |
| -------------------- | --------------------------------------- |
| `getrawtransaction`  | Raw transaction by txid (verbosity 0/1) |
| `sendrawtransaction` | Submit a signed raw transaction         |
| `gettxout`           | UTXO lookup (transparent outputs only)  |

#### Mempool

| Method           | Description                      |
| ---------------- | -------------------------------- |
| `getrawmempool`  | List transactions in the mempool |
| `getmempoolinfo` | Mempool statistics (Zebra 3.0.0) |

#### Mining & Difficulty

| Method             | Description                                     |
| ------------------ | ----------------------------------------------- |
| `getblocktemplate` | Template for constructing candidate blocks      |
| `getmininginfo`    | Mining status and current difficulty            |
| `getnetworksolps`  | Estimated network Equihash solutions per second |

#### Peer Info

| Method        | Description                                             |
| ------------- | ------------------------------------------------------- |
| `getpeerinfo` | Connected peer details (extended fields in Zebra 3.0.0) |

#### Address Queries (Zcash Extensions)

| Method              | Description                                   |
| ------------------- | --------------------------------------------- |
| `getaddressbalance` | Transparent balance for one or more addresses |
| `getaddresstxids`   | Transaction IDs involving an address          |
| `getaddressutxos`   | UTXOs held by an address                      |

#### Address & Script Utilities

| Method              | Description                                     |
| ------------------- | ----------------------------------------------- |
| `validateaddress`   | Validate a transparent (t1/t3) address          |
| `z_validateaddress` | Validate a Sapling, Orchard, or Unified address |

#### Shielded Pool

| Method                   | Description                                              |
| ------------------------ | -------------------------------------------------------- |
| `z_getsubtreesbyindex`   | Subtree roots by index (Ironwood NCT)                    |
| `z_listunifiedreceivers` | Decompose a Unified Address into its component receivers |

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)
