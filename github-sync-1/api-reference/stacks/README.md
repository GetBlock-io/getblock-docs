---
description: >-
  GetBlock provides fast and reliable access to Stacks nodes via the REST API.
  Connect to the Stacks network without running your own infrastructure.
---

# Stacks

Stacks is a Bitcoin Layer-2 that brings smart contracts and decentralized apps to Bitcoin: its Clarity contracts can read Bitcoin state, and through the Nakamoto release and sBTC, Stacks transactions settle with Bitcoin finality. The native token is STX, denominated in microSTX (1 STX = 1,000,000 microSTX). GetBlock exposes Stacks through its node RPC API: a path-addressed HTTP + JSON interface under `/v2` where reads use GET and transaction broadcast, read-only contract calls, and fee estimation use POST.

### Key Features

* **Bitcoin Layer-2**: Clarity smart contracts that can read Bitcoin state; transactions anchor to Bitcoin
* **Node RPC API**: A path-addressed `/v2` HTTP + JSON interface — not a JSON-RPC envelope and not `eth_*`; the resource is chosen by the URL path
* **STX / microSTX**: Balances and fees are denominated in microSTX (1 STX = 1,000,000 microSTX); account balances are returned hex-encoded
* **Clarity Contracts**: Read contract interfaces and source, call read-only functions, and read maps, variables, and constants
* **Decidable Language**: Clarity is interpreted and decidable — no compiler, and contract source is readable on-chain
* **Build-Sign-Broadcast**: Estimate fees, sign client-side, then broadcast the serialized transaction

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE REST API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Stacks' node RPC is documented by the Stacks project; the canonical specification is the stacks-core node RPC at_ [_docs.stacks.co_](https://docs.stacks.co/)_, and developer guidance is published by Hiro at_ [_docs.hiro.so_](https://docs.hiro.so/)_. This reference documents the node's developer-facing `/v2` read and broadcast surface; internal P2P, consensus, and StackerDB routes are omitted._
{% endhint %}

### Network Information

| Property          | Value                                |
| ----------------- | ------------------------------------ |
| Network Name      | Stacks Mainnet                       |
| Native Currency   | STX (1 STX = 1,000,000 microSTX)     |
| Settlement Layer  | Bitcoin                              |
| Contract Language | Clarity                              |
| API               | Node RPC API (HTTP + JSON) under /v2 |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="New York, USA" %}
```
https://shared.us-east-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}

{% tab title="Singapore, Singapore" %}
```
https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

### Supported Networks

| Network | API        | Frankfurt, Germany | New York, USA | Singapore, Singapore |
| ------- | ---------- | ------------------ | ------------- | -------------------- |
| Mainnet | REST (/v2) | ✅                  | ✅             | ✅                    |

### Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir stacks-api-quickstart && cd stacks-api-quickstart && npm init --yes
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

{% code title="index.js" overflow="wrap" %}
```javascript
const axios = require('axios');

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://shared.us-east-1.getblock.io/<ACCESS_TOKEN>/v2/info',
  headers: { 
    'Accept': 'application/json'
  }
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
    "peer_version": 402653200,
    "pox_consensus": "7501a38adfc1a5a206ae809b3dc2d494a484e6bd",
    "burn_block_height": 971693,
    "stable_pox_consensus": "b431a79173d2b5e645d3239310cf4fdc2917c9b0",
    "stable_burn_block_height": 971686,
    "server_version": "stacks-node 4.0.4 (10474cdec9, release build, linux [x86_64])",
    "network_id": 1,
    "parent_network_id": 3652501241,
    "stacks_tip_height": 9223854,
    "stacks_tip": "6b691521b70c9debdb7621fb6ce25cf87925f62d44df34fbeadf62339920254c",
    "stacks_tip_consensus_hash": "50649ccae8233f575361f3a92a253eafa7873b7b",
    "genesis_chainstate_hash": "74237aa39aa50a83de11a4f53e9d3bb7d43461d1de9873f402e5453ae60bc59b",
    "unanchored_tip": null,
    "unanchored_seq": null,
    "tenure_height": 259365,
    "exit_at_block_height": null,
    "is_fully_synced": true,
    "node_public_key": "032e39d727383db1f7f66c7060615f42d42418f3241d5332eb036938a1d4b52b2d",
    "node_public_key_hash": "8272bf1324a0e1df16e64ce32d3fa53f1ccdb984",
    "last_pox_anchor": {
        "anchor_block_hash": "cf90dccf666f393d0e45cfcc0e1dc363f48c95a357eb8030033f1b0b4b5670a6",
        "anchor_block_txid": "55268154eb7b051c17c092635de8fce769c6ca14548cd967d9b2ee74a9a3f39c"
    },
    "stackerdbs": []
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
mkdir stacks-api-quickstart && cd stacks-api-quickstart
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

url = "https://shared.us-east-1.getblock.io/<ACCESS_TOKEN>/v2/info"

payload = {}
headers = {
  'Accept': 'application/json'
}

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

## Available REST Endpoints

### Node & Network

| Endpoint | Method | Description                                                                                                                                                                             |
| -------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| info     | GET    | Returns core node information: peer and server version, network ID, the current burnchain (Bitcoin) block height, the Stacks chain tip height and hash, tenure height, and sync status. |
| pox      | GET    | Returns Proof-of-Transfer (PoX) details: the active PoX contract, cycle lengths, total liquid supply, and the current and next reward cycles with their stacking thresholds.            |

### Accounts & Fees

| Endpoint         | Method | Description                                                                                                                                                                                                      |
| ---------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accounts         | GET    | Returns an account's STX balance, locked (stacked) balance, unlock height, and next nonce. Balances are hex-encoded microSTX. A principal is a standard address or a contract principal (address.contract-name). |
| fees-transfer    | GET    | Returns the estimated fee rate for a standard STX token-transfer transaction, in microSTX per byte.                                                                                                              |
| fees-transaction | POST   | Returns approximate fees for a given transaction payload, as low/middle/high estimations, based on the serialized payload and its estimated length.                                                              |

### Transactions

| Endpoint     | Method | Description                                                                                                                                                                                                                          |
| ------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| transactions | POST   | Broadcasts a signed, serialized transaction to the network. The body is the raw transaction bytes (hex string with application/json, or raw bytes with application/octet-stream). On acceptance the node returns the transaction ID. |

### Contracts

| Endpoint           | Method | Description                                                                                                                                                                      |
| ------------------ | ------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| contract-interface | GET    | Returns the interface (ABI) of a deployed Clarity contract: its public and read-only functions, data variables, maps, fungible and non-fungible tokens, and the Clarity version. |
| contract-source    | GET    | Returns the Clarity source code of a deployed contract, the block height at which it was published, and (optionally) a MARF proof.                                               |
| call-read          | POST   | Calls a read-only Clarity function and returns its result, without creating a transaction. Arguments are hex-encoded Clarity values; the result is a hex-encoded Clarity value.  |

### Contract State

| Endpoint     | Method | Description                                                                                                                                                                                         |
| ------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| map-entry    | POST   | Returns the value stored at a key in a contract's data map. The request body is the hex-encoded Clarity value of the key; the response is the hex-encoded Clarity value and an optional MARF proof. |
| constant-val | GET    | Returns the value of a constant defined in a contract, as a hex-encoded Clarity value.                                                                                                              |
| data-var     | GET    | Returns the current value of a contract data variable, as a hex-encoded Clarity value, with an optional MARF proof.                                                                                 |
| traits       | GET    | Reports whether a contract implements a given trait, identified by the trait's deployer, contract, and trait name.                                                                                  |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Stacks Documentation](https://docs.stacks.co/)
* [Stacks Node RPC Reference](https://docs.stacks.co/reference/api)
* [Hiro Developer Docs](https://docs.hiro.so/)
* [Stacks Explorer](https://explorer.hiro.so/)
