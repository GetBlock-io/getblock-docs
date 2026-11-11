---
description: >-
  GetBlock provides fast and reliable access to Avail nodes via JSON-RPC API.
  Connect to the Avail network without running your own infrastructure.
---

# Avail

Avail is a modular blockchain that provides scalable, trust-minimized data availability for rollups and other execution layers. It separates data availability from execution: rollups publish their transaction data to Avail, and Avail guarantees — through erasure coding, KZG polynomial commitments, and data-availability sampling by light clients — that the data is verifiably available without any single party having to store or re-execute it.&#x20;

The Avail Node is a Substrate-based client secured by BABE block production and GRANDPA finality, with a native AVAIL token used for data-availability fees and staking. Applications interact with Avail over the Substrate JSON-RPC (system, chain, state, and author methods), Avail's own Kate RPC for data-availability queries and proofs, and WebSocket subscriptions for live updates.

## Key Features

* **Data Availability Layer**: Publishes and guarantees the availability of rollup data without re-execution
* **KZG Commitments + Sampling**: Erasure-coded data with KZG commitments lets light clients verify availability by sampling a few cells
* **Substrate (BABE + GRANDPA)**: Substrate-based node with deterministic GRANDPA finality
* **Kate RPC**: Avail-specific `kate_*` methods for block dimensions, matrix rows, cell proofs, and data-inclusion proofs
* **Trust-Minimized Bridging**: Data-inclusion proofs (via `kate_queryDataProof`) let bridges prove availability on Ethereum
* **AVAIL Token**: Used for data-availability fees and staking; denominated to 18 decimals

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Avail's Substrate JSON-RPC follows the Substrate/Polkadot RPC specification, and the Kate RPC is defined by the Avail Node implementation; both are documented at_ [_docs.availproject.org_](https://docs.availproject.org/)_._
{% endhint %}

## Interfaces

| Interface                | Transport                             | Use it for                                                                                             |
| ------------------------ | ------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| JSON-RPC                 | Substrate JSON-RPC 2.0 over HTTP      | Request/response queries (system, chain, state, author, payment) and extrinsic submission              |
| Data Availability (Kate) | Substrate JSON-RPC 2.0 over HTTP      | Avail-specific `kate_*` methods: block dimensions, matrix rows, cell proofs, and data-inclusion proofs |
| WebSocket                | Substrate JSON-RPC 2.0 over WebSocket | The same methods plus pub-sub subscriptions (new/finalized heads, storage changes, extrinsic status)   |

## Interface Endpoints

{% tabs %}
{% tab title="JSON-RPC (HTTP)" %}
{% code overflow="wrap" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endcode %}
{% endtab %}

{% tab title="WebSocket (WSS)" %}
```bash
wss://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

## Network Information

| Property        | Value                                       |
| --------------- | ------------------------------------------- |
| Network Name    | Avail                                       |
| Native Currency | AVAIL                                       |
| Token Decimals  | 18                                          |
| SS58 Format     | 42                                          |
| Framework       | Substrate (data-availability layer)         |
| Consensus       | BABE block production with GRANDPA finality |
| Block Time      | \~20 seconds                                |
| Finality        | Deterministic (GRANDPA)                     |

## Supported Networks

| Network | JSON-RPC (HTTP) | WebSocket (WSS) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | --------------- | --------------- | ------------------ | ------------- | -------------------- |
| Mainnet | ✅               | ✅               | ✅                  | ✅             | ✅                    |

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir avail-api-quickstart && cd avail-api-quickstart && npm init --yes
```
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Add code

{% code title="index.js" %}
```javascript
const axios = require('axios');
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "system_chain",
  "params": []
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>',
  headers: { 
    'Content-Type': 'application/json', 
    'Accept': 'application/json'
  },
  data : data
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
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
{
    "jsonrpc": "2.0",
    "result": "Avail DA Mainnet",
    "id": "getblock.io"
}
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
mkdir avail-api-quickstart && cd avail-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endstep %}

{% step %}
### Install dependency

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

url = "https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>"

payload = json.dumps({
  "jsonrpc": "2.0",
  "id": "getblock.io",
  "method": "system_chain",
  "params": []
})
headers = {
  'Content-Type': 'application/json',
  'Accept': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

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

## APIs

* [Substrate JSON-RPC API](json-rpc-api/) — request/response methods (system, chain, state, author, payment), over HTTP and WebSocket
* [Data Availability (Kate) API](data-availability-api/) — Avail-specific DA methods for matrix dimensions, rows, cell proofs, and data-inclusion proofs
* [WebSocket Subscriptions](websocket-subscriptions-api/) — pub-sub streams (new heads, finalized heads, storage, extrinsic status)

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Avail Documentation](https://docs.availproject.org/)
* [Substrate JSON-RPC](https://polkadot.js.org/docs/substrate/rpc/)
* [Avail Kate RPC](https://docs.availproject.org/docs/build-with-avail/interact-with-avail-da/reading-data)
* [Avail Explorer (Subscan)](https://avail.subscan.io/)
