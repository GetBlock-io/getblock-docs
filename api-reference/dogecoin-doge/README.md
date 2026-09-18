---
description: >-
  GetBlock provides fast and reliable access to Dogecoin nodes via JSON-RPC API.
  Connect to the Dogecoin network without running your own infrastructure.
---

# Dogecoin (DOGE)

### Overview

Dogecoin is a decentralized, peer-to-peer cryptocurrency that was created in December 2013 as a lighthearted alternative to Bitcoin. Originally started as a joke based on the popular "Doge" Shiba Inu meme, Dogecoin has grown into a legitimate digital currency with a vibrant, philanthropic community.

### Key Features

* **Fast Transactions**: 1-minute block time (vs Bitcoin's 10 minutes)
* **Low Fees**: Transaction fees are significantly lower than Bitcoin
* **Inflationary Supply**: No maximum supply cap, with \~5 billion new DOGE mined annually
* **Scrypt Algorithm**: Uses Scrypt proof-of-work, merged mining with Litecoin
* **Active Community**: Large, engaged community focused on tipping and charitable giving
* **Wide Adoption**: Accepted by many merchants and supported by major exchanges

## Network Information

| Property        | Value                     |
| --------------- | ------------------------- |
| Network Name    | Dogecoin Mainnet          |
| Currency Symbol | DOGE                      |
| Block Time      | \~1 minute                |
| Consensus       | Proof of Work (Scrypt)    |
| RPC Port        | 22555                     |
| P2P Port        | 22556                     |
| Address Prefix  | D (mainnet)               |
| Smallest Unit   | 1 Koinu = 0.00000001 DOGE |

## Base URL

{% tabs %}
{% tab title="Franfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io
```
{% endtab %}
{% endtabs %}

### Available API Interface

| Network | JSON RPC | REST | Blockbook (REST) | Blockbook (WebSocket) |
| ------- | -------- | ---- | ---------------- | --------------------- |
| Mainnet | ✅        | ✅    | ✅                | ✅                     |

{% hint style="info" %}
JSON-RPC is the Dogecoin Core node interface. Blockbook is a separate add-on providing an address- and xpub-indexed view of the chain, and its REST and WebSocket interfaces are provisioned as their own endpoints with their own URLs. A JSON-RPC endpoint does not answer address, UTXO, or wallet queries, and the Dogecoin Core wallet RPCs such as `listunspent` are disabled on shared nodes. See the [Blockbook add-on](../../add-ons/blockbook.md).
{% endhint %}

## Quickstart

In this section, you will learn how to make your first call with either:

* Axios
* Python

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

{% code overflow="wrap" %}
```bash
mkdir dogecoin-api-quickstart && cd dogecoin-api-quickstart && npm init --yes
```
{% endcode %}
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
import axios from 'axios'
let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "getinfo",
    "params": [],
    "id": "getblock.io"
});

let config = {
  method: "post",
  maxBodyLength: Infinity,
  url: "https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>",
  headers: {
    "Content-Type": "application/json",
  },
  data: data,
};

axios
  .request(config)
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
    "result": {
        "version": 1140900,
        "protocolversion": 70015,
        "blocks": 6010614,
        "timeoffset": 0,
        "connections": 8,
        "proxy": "",
        "difficulty": 45990648.91368412,
        "testnet": false,
        "paytxfee": 0.01000000,
        "relayfee": 0.00100000,
        "errors": ""
    },
    "error": null,
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

{% code overflow="wrap" %}
```bash
mkdir dogecoin-api-quickstart && cd dogecoin-api-quickstart
python -m venv venv && source venv/bin/activate
```
{% endcode %}
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

## Available API Methods

Dogecoin is documented across three interfaces, each provisioned as its own endpoint.

1. [Dogecoin JSON-RPC API](dogecoin-json-rpc-api/): The Dogecoin Core node interface: blocks, raw transactions, individual unspent outputs, and node and mining state.
2. [Dogecoin Blockbook REST API](dogecoin-blockbook-rest-api/): Address- and xpub-indexed queries over HTTP: balances, transaction history, unspent outputs, balance history, and fiat rates.
3. [Dogecoin Blockbook (WebSocket) API](dogecoin-blockbook-websocket-api/): The same indexed queries over a persistent connection, plus subscriptions to new blocks and to activity on a set of addresses.

{% hint style="warning" %}
Two Dogecoin behaviours catch integrations out. A **one-block fee target returns `-1`**, which becomes a negative fee if multiplied out unchecked. And blocks target **one minute**, so a confirmation depth copied from a Bitcoin integration settles in roughly a tenth of the wall-clock time. Both are covered in the [Blockbook REST reference](dogecoin-blockbook-rest-api/).
{% endhint %}

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Dogecoin Official Documentation](https://dogecoin.com/)
* [Dogecoin GitHub Repository](https://github.com/dogecoin/dogecoin)
* [Dogecoin Block Explorer](https://dogechain.info/)
