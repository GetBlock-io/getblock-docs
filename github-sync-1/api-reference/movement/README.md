---
description: >-
  GetBlock provides fast and reliable access to Movement nodes via JSON-RPC API.
  Connect to the Movement network without running your own infrastructure.
---

# Movement

Movement is a Move-based Layer-1 network that runs the MoveVM with an Aptos-compatible execution layer. State is modeled as Move resources stored under accounts, smart contracts are written in Move, and the native token is MOVE, denominated in Octas (1 MOVE = 100,000,000 Octas). GetBlock exposes Movement through its Aptos-compatible REST API: a path-addressed HTTP + JSON interface under `/v1` where reads use GET and transaction submission, simulation, and view calls use POST.

## Key Features

* **MoveVM + Resources**: State is typed Move resources owned by accounts, not account balances in a single trie
* **Aptos-Compatible REST**: A path-addressed `/v1` HTTP API — not a JSON-RPC envelope and not `eth_*`
* **MOVE / Octas**: Balances and gas are denominated in Octas (1 MOVE = 100,000,000 Octas)
* **Versions & Blocks**: Chain progress is tracked by ledger version and block height
* **Move Modules**: Published modules expose entry functions and read-only view functions
* **Build-Sign-Submit**: Encode a signing message, sign it, and submit the signed transaction

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE REST API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. Movement's node API is Aptos-compatible; the canonical specification is the Aptos Node REST API, and Movement-specific guidance is published at_ [_docs.movementnetwork.xyz_](https://docs.movementnetwork.xyz/api/node)_. This reference documents the Move Layer-1 REST API; a separate EVM interface, if offered, would be documented on its own._
{% endhint %}

## Network Information

| Property          | Value                                         |
| ----------------- | --------------------------------------------- |
| Network Name      | Movement Mainnet                              |
| Chain ID          | 126                                           |
| Native Currency   | MOVE (1 MOVE = 100,000,000 Octas)             |
| Protocol          | MoveVM (Aptos-compatible)                     |
| Contract Language | Move                                          |
| API               | Aptos-compatible REST (HTTP + JSON) under /v1 |

## Base URL

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

## Quickstart

{% tabs %}
{% tab title="Javascript(Axios)" %}
{% stepper %}
{% step %}
### Setup project

```bash
mkdir move-api-quickstart && cd move-api-quickstart  && npm init --yes
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
  url: 'https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>/v1/accounts/0x1/events/0?start=0&limit=25',
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
[
    {
        "version": "154711",
        "guid": {
            "creation_number": "0",
            "account_address": "0x1"
        },
        "sequence_number": "0",
        "type": "0x1::account::CoinRegisterEvent",
        "data": {
            "type_info": {
                "account_address": "0x327aab82300f1f7806de9dee47d0f8f553b0c8e0acb2203f38f4e3f4d97d099a",
                "module_name": "0x73776170",
                "struct_name": "0x4c50546f6b656e3c3078313a3a6170746f735f636f696e3a3a4170746f73436f696e2c203078613965333930323663346137393330373862656332646461303563306434366131643936313134356433643636366562363364313530666466343462366363663a3a746573745f636f696e3a3a54657374436f696e3e"
            }
        }
    },
    {
        "version": "154983",
        "guid": {
            "creation_number": "0",
            "account_address": "0x1"
        },
        "sequence_number": "1",
        "type": "0x1::account::CoinRegisterEvent",
        "data": {
            "type_info": {
                "account_address": "0x1",
                "module_name": "0x6170746f735f636f696e",
                "struct_name": "0x4170746f73436f696e"
            }
        }
    }
]
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
mkdir move-api-quickstart && cd move-api-quickstart
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

url = "https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>/v1/accounts/0x1/events/0?start=0&limit=25"

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

### Ledger & Node

| Endpoint           | Method | Description                                                                                                                                         |
| ------------------ | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| ledger-info        | GET    | Returns the current ledger information for the node: chain ID, current epoch, ledger version, oldest retained version, and current block height     |
| healthy            | GET    | Returns a health check for the node                                                                                                                 |
| spec _(dedicated)_ | GET    | Returns the node's own OpenAPI specification                                                                                                        |
| estimate-gas-price | GET    | Returns the network's current gas unit price estimate, with deprioritized and prioritized tiers, so a client can set a competitive gas\_unit\_price |

### Accounts

| Endpoint             | Method | Description                                                                                                                                                              |
| -------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| account              | GET    | Returns the core on-chain data of an account: its current sequence number (used as the nonce for the next transaction) and its authentication key                        |
| account-resources    | GET    | Returns all Move resources stored under an account, each with its fully-qualified type and decoded data — including the coin store that holds the account's MOVE balance |
| account-resource     | GET    | Returns a single Move resource under an account, addressed by its fully-qualified type                                                                                   |
| account-modules      | GET    | Returns all Move modules published under an account, each with its bytecode and ABI                                                                                      |
| account-module       | GET    | Returns a single Move module published under an account, by module name, with its bytecode and ABI                                                                       |
| account-transactions | GET    | Returns the transactions sent by an account (by its sequence numbers), paginated — an account's outbound transaction history                                             |

### Blocks

| Endpoint         | Method | Description                                                                                                                                     |
| ---------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| block-by-height  | GET    | Returns a block by its height, including its first and last transaction versions, timestamp, and (optionally) the full transactions it contains |
| block-by-version | GET    | Returns the block that contains a given ledger (transaction) version                                                                            |

### Transactions

| Endpoint                  | Method | Description                                                                                                                                |
| ------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| transactions              | GET    | Returns a paginated list of committed transactions, newest-relative to a starting ledger version, with their payloads, events, and effects |
| transaction-by-hash       | GET    | Returns a single transaction by its hash, including its payload, events, state changes, and VM status                                      |
| wait-transaction-by-hash  | GET    | Returns a transaction by hash, blocking until it is committed (or a timeout)                                                               |
| transaction-by-version    | GET    | Returns a single committed transaction by its ledger version                                                                               |
| submit-transaction        | POST   | Submits a signed transaction for execution and returns the pending transaction (including its hash)                                        |
| submit-batch-transactions | POST   | Submits a batch of signed transactions in one call                                                                                         |
| simulate-transaction      | POST   | Simulates a transaction without committing it and returns its predicted effects, gas usage, and success status                             |
| encode-submission         | POST   | Returns the BCS-encoded signing message for a transaction, the bytes a client signs before submitting                                      |

### Execution & State

| Endpoint       | Method | Description                                                                                                                                       |
| -------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| view-function  | POST   | Executes a read-only Move view function against the current (or a historical) ledger state and returns its result, without creating a transaction |
| table-item     | POST   | Returns a single item from a Move table by its key                                                                                                |
| raw-table-item | POST   | Returns a single table item by its raw (BCS-encoded) key — the lower-level counterpart to get table item, used when the key is already serialized |

### Events

| Endpoint                  | Method | Description                                                                                                                                  |
| ------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| events-by-creation-number | GET    | Returns events from an event stream identified by the account address and the stream's creation number, paginated                            |
| events-by-handle          | GET    | Returns events from an event handle addressed by the account, the handle's resource struct type, and the field within that struct, paginated |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Movement Node API Documentation](https://docs.movementnetwork.xyz/api/node)
* [Aptos Node REST API](https://api.mainnet.aptoslabs.com/v1/spec)
* [Movement Explorer](https://explorer.movementnetwork.xyz/)
* [Movement Website](https://movementnetwork.xyz/)
