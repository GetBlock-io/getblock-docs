---
description: >-
  Algorand API reference for accessing Algorand nodes through GetBlock's Algod
  and Indexer REST APIs.
---

# Algorand (ALGO)

Algorand is a Layer 1 blockchain secured by Pure Proof-of-Stake, designed for instant, single-block finality with low, fixed fees. Proposed by Turing Award winner Silvio Micali and launched in 2019, it uses a verifiable random function committee to reach agreement, so blocks (called rounds) are final the moment they are committed — there are no reorganizations. Algorand has native tokenization through Algorand Standard Assets (ASAs) and stateful smart contracts (applications) that run TEAL on the Algorand Virtual Machine. Its native token is ALGO, denominated in microAlgos (10^-6 ALGO). GetBlock serves Algorand over two REST interfaces: algod (the node daemon) and the Indexer (archival search).

## Key Features

* **Pure Proof-of-Stake**: A VRF-based committee reaches agreement with instant, single-round finality
* **No Reorgs**: Once a round is committed it is final, so confirmations are immediate
* **Algorand Standard Assets**: Native tokenization (fungible and non-fungible) without smart contracts
* **AVM Smart Contracts**: Stateful applications and stateless logic signatures written in TEAL
* **Low, Fixed Fees**: A minimum fee of 1,000 microAlgos per transaction under normal load
* **Two REST APIs**: algod for live node state and submission; the Indexer for historical search

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE REST API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specifications for the algod and Indexer REST APIs are maintained and published by the Algorand Foundation and the go-algorand project at_ [_developer.algorand.org_](https://developer.algorand.org/)_. Those OpenAPI specifications are the authoritative references._
{% endhint %}

## Network Information

| Property        | Value                                    |
| --------------- | ---------------------------------------- |
| Network Name    | Algorand Mainnet                         |
| Native Currency | ALGO (1 ALGO = 1,000,000 microAlgos)     |
| Consensus       | Pure Proof-of-Stake                      |
| Finality        | Instant (single round)                   |
| Address Format  | 58-character base32                      |
| Block Time      | \~2.8 seconds                            |
| Node Software   | go-algorand (algod) and Algorand Indexer |

## Interfaces

| Interface      | Transport                           | Use it for                                                                                              |
| -------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------- |
| algod (REST)   | HTTP REST over the node daemon      | Live node status, blocks, current account/asset/app state, suggested params, and transaction submission |
| Indexer (REST) | HTTP REST over the Indexer database | Archival, filtered search of transactions, accounts, assets, applications, and blocks                   |

Use **algod** to read the current state and submit transactions, and the **Indexer** to search history with rich filters and pagination. Each is provisioned as its own endpoint in the GetBlock dashboard.

## Interface Endpoints

{% tabs %}
{% tab title="algod (REST)" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Indexer (REST)" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Replace `<ACCESS-TOKEN>` with the token from the GetBlock dashboard. algod and the Indexer are separate endpoints — select the interface when you create the endpoint. Append the paths shown on each method page (for example `v2/status` or `v2/transactions`).
{% endhint %}

## Supported Networks

| Network | algod (REST) | Indexer (REST) | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | ------------ | -------------- | ------------------ | ------------- | -------------------- |
| Mainnet | ✅            | ✅              | ✅                  | ❌             | ❌                    |

## Quickstart

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

{% code overflow="wrap" %}
```bash
mkdir algorand-quickstart && cd algorand-quickstart && npm init --yes
```
{% endcode %}
{% endstep %}

{% step %}
### Install dependency

```bash
npm install axios
```
{% endstep %}

{% step %}
### Add code

{% code title="index.js" overflow="wrap" %}
```javascript
import axios from "axios";

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>/v2/status',
  headers: { }
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
    "catchpoint": "",
    "catchpoint-acquired-blocks": 0,
    "catchpoint-processed-accounts": 0,
    "catchpoint-processed-kvs": 0,
    "catchpoint-total-accounts": 0,
    "catchpoint-total-blocks": 0,
    "catchpoint-total-kvs": 0,
    "catchpoint-verified-accounts": 0,
    "catchpoint-verified-kvs": 0,
    "catchup-time": 0,
    "last-catchpoint": "",
    "last-round": 65020113,
    "last-version": "https://github.com/algorandfoundation/specs/tree/268b63433a907455d439995bf916f6b296018f4f",
    "next-version": "https://github.com/algorandfoundation/specs/tree/268b63433a907455d439995bf916f6b296018f4f",
    "next-version-round": 65020114,
    "next-version-supported": true,
    "stopped-at-unsupported-round": false,
    "time-since-last-round": 310270888
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
mkdir algorand-quickstart && cd algorand-quickstart
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

{% code title="main.py" overflow="wrap" %}
```python
import requests

url = "https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>/v2/status"

payload = {}
headers = {}

response = requests.request("GET", url, headers=headers, data=payload)

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

* [algod REST API](algod-rest-api/) — the node daemon: status, blocks, accounts, assets, applications, suggested params, submission, and TEAL compile
* [Indexer REST API](indexer-rest-api/) — archival search over transactions, accounts, assets, applications, and blocks

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

### See Also

* [Algorand Developer Portal](https://developer.algorand.org/)
* [algod REST API reference](https://developer.algorand.org/docs/rest-apis/algod/)
* [Indexer REST API reference](https://developer.algorand.org/docs/rest-apis/indexer/)
* [Pera Explorer](https://explorer.perawallet.app/)
