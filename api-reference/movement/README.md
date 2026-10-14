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

| Endpoint                                                                     | Method | Description                                                                                                                                         |
| ---------------------------------------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| [ledger-info](/broken/pages/56d50e9b521a1238ce99e4a01a8286961ea14b7a)        | GET    | Returns the current ledger information for the node: chain ID, current epoch, ledger version, oldest retained version, and current block height     |
| [healthy](/broken/pages/5491fa9722f6014a3842f6a3280f0b2cb5127a3f)            | GET    | Returns a health check for the node                                                                                                                 |
| [spec](/broken/pages/cbd62d8ddde0c072ff214f73c82a34319d7e37ea) _(dedicated)_ | GET    | Returns the node's own OpenAPI specification                                                                                                        |
| [estimate-gas-price](/broken/pages/efc229d6caf353485c95b119ed2ff27d487341dd) | GET    | Returns the network's current gas unit price estimate, with deprioritized and prioritized tiers, so a client can set a competitive gas\_unit\_price |

### Accounts

| Endpoint                                                                       | Method | Description                                                                                                                                                              |
| ------------------------------------------------------------------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [account](/broken/pages/41104e17259686896dcf9471ee0c7f538826b232)              | GET    | Returns the core on-chain data of an account: its current sequence number (used as the nonce for the next transaction) and its authentication key                        |
| [account-resources](/broken/pages/8512e6aff2625dda271caa72893f72ba5118a932)    | GET    | Returns all Move resources stored under an account, each with its fully-qualified type and decoded data — including the coin store that holds the account's MOVE balance |
| [account-resource](/broken/pages/d7ec0020767e7078a1312b535d812ec01e6b2279)     | GET    | Returns a single Move resource under an account, addressed by its fully-qualified type                                                                                   |
| [account-modules](/broken/pages/0f3ec6aa97bbfc40e4d4570fbccdcd28894d3717)      | GET    | Returns all Move modules published under an account, each with its bytecode and ABI                                                                                      |
| [account-module](/broken/pages/a3ca749facf7ddd0c4bb2a85ff2a41ef29d420e0)       | GET    | Returns a single Move module published under an account, by module name, with its bytecode and ABI                                                                       |
| [account-transactions](/broken/pages/3e15c0b656fde2808765f2ed377522c61def2466) | GET    | Returns the transactions sent by an account (by its sequence numbers), paginated — an account's outbound transaction history                                             |

### Blocks

| Endpoint                                                                   | Method | Description                                                                                                                                     |
| -------------------------------------------------------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| [block-by-height](/broken/pages/cd24a1eb8c3faf17c6a6514cc5ba22bb7efd368d)  | GET    | Returns a block by its height, including its first and last transaction versions, timestamp, and (optionally) the full transactions it contains |
| [block-by-version](/broken/pages/b9f180147ada0353b566a8543a2e73f26e9381b9) | GET    | Returns the block that contains a given ledger (transaction) version                                                                            |

### Transactions

| Endpoint                                                                            | Method | Description                                                                                                                                |
| ----------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| [transactions](/broken/pages/6dc86f9faa107c54e4ae97f1d1555ab0c2ec750c)              | GET    | Returns a paginated list of committed transactions, newest-relative to a starting ledger version, with their payloads, events, and effects |
| [transaction-by-hash](/broken/pages/8af36030ec7f1c5005e9ad947a0d94b08d8a6d4a)       | GET    | Returns a single transaction by its hash, including its payload, events, state changes, and VM status                                      |
| [wait-transaction-by-hash](/broken/pages/15d1178b419f7820d64153c289fa5e936f74235a)  | GET    | Returns a transaction by hash, blocking until it is committed (or a timeout)                                                               |
| [transaction-by-version](/broken/pages/dbc7970f4e07ba5449d91895ccfa8f72568bf094)    | GET    | Returns a single committed transaction by its ledger version                                                                               |
| [submit-transaction](/broken/pages/1d6d614277fb0c4bbe89947952aec18d8ac50e74)        | POST   | Submits a signed transaction for execution and returns the pending transaction (including its hash)                                        |
| [submit-batch-transactions](/broken/pages/e3953ba762897515a25001e1e2a5e04eb7679b2e) | POST   | Submits a batch of signed transactions in one call                                                                                         |
| [simulate-transaction](/broken/pages/a1f2a0367949988e39f2395267f475814485dbd4)      | POST   | Simulates a transaction without committing it and returns its predicted effects, gas usage, and success status                             |
| [encode-submission](/broken/pages/4c683d1e165032d7fc31a03a6b55b82f512779b2)         | POST   | Returns the BCS-encoded signing message for a transaction, the bytes a client signs before submitting                                      |

### Execution & State

| Endpoint                                                                 | Method | Description                                                                                                                                       |
| ------------------------------------------------------------------------ | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| [view-function](/broken/pages/1b9b36ba5ebbd4c19a14db6aee9fdfb6d47e03e8)  | POST   | Executes a read-only Move view function against the current (or a historical) ledger state and returns its result, without creating a transaction |
| [table-item](/broken/pages/04d7de7460abf3398772ac8172dbe2f60fa62f8f)     | POST   | Returns a single item from a Move table by its key                                                                                                |
| [raw-table-item](/broken/pages/2ca6d7ec412eace82c0ae111391282bb70396d9a) | POST   | Returns a single table item by its raw (BCS-encoded) key — the lower-level counterpart to get table item, used when the key is already serialized |

### Events

| Endpoint                                                                            | Method | Description                                                                                                                                  |
| ----------------------------------------------------------------------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| [events-by-creation-number](/broken/pages/c5a038907871c88d71e781e30ad2d219d690be76) | GET    | Returns events from an event stream identified by the account address and the stream's creation number, paginated                            |
| [events-by-handle](/broken/pages/e9384d2f3536f44a580ab76c03829bca23dccf52)          | GET    | Returns events from an event handle addressed by the account, the handle's resource struct type, and the field within that struct, paginated |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Movement Node API Documentation](https://docs.movementnetwork.xyz/api/node)
* [Aptos Node REST API](https://api.mainnet.aptoslabs.com/v1/spec)
* [Movement Explorer](https://explorer.movementnetwork.xyz/)
* [Movement Website](https://movementnetwork.xyz/)
