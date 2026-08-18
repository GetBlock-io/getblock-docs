---
description: >-
  GetBlock provides fast, reliable access to Cardano nodes via Ogmios JSON-RPC
  and the Rosetta API. Connect to the Cardano network without running your own
  infrastructure.
---

# Cardano(ADA)

Cardano is a proof-of-stake Layer 1 blockchain that uses the Extended UTXO (EUTXO) accounting model and the Ouroboros consensus protocol. It supports native assets that move without smart contracts and Plutus scripts for programmable validation. GetBlock exposes Cardano through three interfaces: the Rosetta API, Ogmios over JSON-RPC, and Ogmios over WebSocket.

### Key Features

* **EUTXO Model**: Extends the unspent-output model with scripts and datums for deterministic validation.
* **Native Assets**: Mints and transfers tokens as first-class ledger objects without contracts.
* **Ouroboros Consensus**: Secures the chain with a peer-reviewed proof-of-stake protocol.
* **Staking and Delegation**: Lets ada holders delegate to stake pools and earn rewards.
* **On-Chain Governance**: Supports the Conway-era governance system of proposals and voting.
* **Plutus Scripts**: Validates spending with scripts whose execution units are metered.
* **Rosetta Interface**: Offers a standardized construction and data API for exchange integration.
* **Ogmios Bridge**: Provides low-level ledger, network, and mempool access over JSON-RPC and WebSocket.

{% hint style="info" %}
_TECHNICAL DISCLAIMER._

_GetBlock's Cardano API reference documentation is provided for informational purposes and developer experience. The canonical specifications are maintained by the Rosetta and Ogmios projects and by Input Output Global. The Rosetta interface follows the Rosetta API specification; the Ogmios interface follows the Ogmios v6 JSON-RPC API._
{% endhint %}

### Network Information

| Property        | Value                            |
| --------------- | -------------------------------- |
| Network Name    | Cardano Mainnet                  |
| Native Currency | ADA (1 ADA = 1,000,000 lovelace) |
| Consensus       | Ouroboros Proof of Stake         |
| Model           | Extended UTXO (EUTXO)            |
| Address Format  | Bech32 (addr, stake)             |

### Base URL

{% tabs %}
{% tab title="Rosetta / Ogmios (JSON_RPC)" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Ogmios (WebSocket)" %}
```bash
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network | Rosetta | Ogmios JSON-RPC | Ogmios WebSocket |
| ------- | ------- | --------------- | ---------------- |
| Mainnet | ✅       | ✅               | ✅                |

### APIs

Cardano is served through three interfaces. Each has its own reference section.

| Interface                               | Transport | Use it for                                                          |
| --------------------------------------- | --------- | ------------------------------------------------------------------- |
| [Rosetta](rosetta-api/)                 | HTTP POST | Standardized data and transaction construction, exchange flows      |
| [Ogmios JSON-RPC](ogmios-json-rpc-api/) | HTTP POST | Ledger and network state queries, transaction submission            |
| [Ogmios WebSocket](./#ogmios-websocket) | WebSocket | Chain synchronization, ledger-state acquisition, mempool monitoring |

### Quickstart

In this section, you will learn how to make your first call with either:

* Javascript(Axios)
* Python

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
#### Setup project

Create and initialize a new project:

```bash
mkdir cardano-api-quickstart
cd cardano-api-quickstart
npm init --yes
```
{% endstep %}

{% step %}
#### Install Axios

```bash
npm install axios
```
{% endstep %}

{% step %}
#### Create file

Create a new file named `index.js`. This is where you will make your first call.
{% endstep %}

{% step %}
#### Set ES module type

Set the ES module `"type": "module"` in your `package.json`.
{% endstep %}

{% step %}
#### Add code

Add the following code to `index.js`:

{% code title="index.js" %}
```javascript
const axios = require('axios');

const url = 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/';

const payload = {
  jsonrpc: '2.0',
  method: 'queryNetwork/blockHeight',
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
#### Run the script

```bash
node index.js
```

Expected output (example):

```json
{
    "jsonrpc": "2.0",
    "method": "queryNetwork/blockHeight",
    "result": 13748077,
    "id": "getblock.io"
}
```
{% endstep %}
{% endstepper %}
{% endtab %}

{% tab title="Python(Request)" %}
{% stepper %}
{% step %}
#### Set up the project directory

```bash
mkdir cardano-api-quickstart
cd cardano-api-quickstart
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
#### Install requests

```bash
pip install requests
```
{% endstep %}

{% step %}
#### Create script

Create a file called `main.py` with the following content:

{% code title="main.py" %}
```python
import requests
import json

url = "https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/"

payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "queryNetwork/blockHeight",
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
#### Run the script

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

### See Also

* [Rosetta API Specification](https://www.rosetta-api.org/docs/welcome.html)
* [Ogmios Documentation](https://ogmios.dev/)
* [Cardano Developer Portal](https://developers.cardano.org/)
