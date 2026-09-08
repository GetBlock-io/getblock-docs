---
description: >-
  Dash API Reference for blockchain access through Dash nodes, including
  JSON-RPC, REST, and Blockbook APIs.
---

# Dash (DASH)

Dash is a Layer 1 proof-of-work blockchain, derived from Bitcoin, built for fast and low-cost payments. Its distinguishing feature is a two-tier network: miners secure the chain with the X11 proof-of-work algorithm, while a layer of collateralized masternodes provides InstantSend (near-instant, locked payments), ChainLocks (protection against deep reorganizations), and on-chain governance. It keeps Bitcoin's UTXO model and a Bitcoin Core-style JSON-RPC interface, and is served by the Dash Core node software. GetBlock exposes Dash over four interfaces: Core JSON-RPC, Bitcoin-style REST, and the Blockbook indexer's REST and WebSocket APIs.

### Key Features

* **Two-Tier Network**: Miners plus collateralized masternodes that provide advanced services
* **InstantSend**: Masternode quorums lock transactions for near-instant, spend-safe confirmation
* **ChainLocks**: Quorum-signed blocks that protect against deep chain reorganizations
* **UTXO Model**: Uses the unspent transaction output model inherited from Bitcoin
* **X11 Proof of Work**: Secured by the chained X11 hashing algorithm
* **Bitcoin Core RPC**: Exposes a JSON-RPC interface familiar to Bitcoin developers, plus Dash extensions
* **Indexer APIs**: Blockbook adds address-indexed REST and WebSocket access on top of the node

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE JSON-RPC API SPECIFICATION._

_GetBlock's RPC API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical and normative specification for Dash Core RPC methods is maintained and published through the official Dash Core documentation, and the Blockbook REST/WebSocket API is specified by the Blockbook project. These resources constitute the authoritative references._
{% endhint %}

### Network Information

| Property        | Value                                            |
| --------------- | ------------------------------------------------ |
| Network Name    | Dash Mainnet                                     |
| Native Currency | DASH (1 DASH = 100,000,000 duffs)                |
| Consensus       | Proof of Work (X11) with a masternode LLMQ layer |
| Address Format  | Base58 (P2PKH starts with `X`, P2SH with `7`)    |
| Model           | UTXO                                             |
| Node Software   | Dash Core                                        |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network | JSON-RPC | REST | Blockbook (REST) | Blockbook (WebSocket) |
| ------- | -------- | ---- | ---------------- | --------------------- |
| Mainnet | ✅        | ✅    | ✅                | ✅                     |

### Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir dash-api-quickstart && cd dash-api-quickstart && npm init --yes
```
{% endstep %}

{% step %}
### Install Axios

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
  method: 'getblockcount',
  params: [],
  id: 'getblock.io'
};

axios.post(url, payload, {
  headers: { 'Content-Type': 'application/json' }
})
.then(response => {
  console.log('Current block height:', response.data.result);
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
Current block height: 2535413
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
mkdir dash-api-quickstart && cd dash-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endstep %}

{% step %}
### Install requests

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

### APIs

* [JSON-RPC API](bitcoin-cash-json-rpc-api/) — Dash Core JSON-RPC (Bitcoin-style + masternode/quorum methods)
* [REST API](dash-rest-api/) — Bitcoin Core-style REST endpoints (`/rest/...`)
* [Blockbook REST API](bitcoin-cash-blockbook-rest-api/) — address-indexed indexer REST (`/api/...`)
* [Blockbook WebSocket API](dash-blockbook-websocket-api/) — indexer WebSocket (queries + subscriptions)

### Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

#### See Also

* [Dash Core Documentation](https://docs.dash.org/)
* [Blockbook API](https://github.com/trezor/blockbook/blob/master/docs/api.md)
* [Dash Block Explorer](https://blockchair.com/dash)
