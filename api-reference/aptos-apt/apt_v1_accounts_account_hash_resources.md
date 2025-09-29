---
description: >-
  This endpoint gets all resources associated with a specific account at the
  latest ledger version. These include authentication key, sequence number, GUID,
  smart contract states, tokens, and more.
---

# /v1/accounts/{account_hash}/resources – Aptos


This endpoint retrieves all resources linked to a specific account at the latest ledger version. These resources include on-chain data such as the authentication key, sequence number, GUIDs, smart contract states, and tokens.

## Supported Networks
- Mainnet

## Parameters

| Parameter       | Data type | Description                                | Required | In    |
|-----------------|-----------|--------------------------------------------|----------|-------|
| `account_hash`  | string    | Aptos account address                      | Yes      | Path  |
| `ledger_version`| string    | The ledger version to get the account state| No       | Query |
| `start`         | string    | The starting point for retrieving resources| No       | Query |
| `limit`         | integer   | The maximum number of resources per request| No       | Query |


## Request Example

**URL**
```bash
https://go.getblock.io//
```
**Example**
```bash

curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/resources?ledger_version=3357308041&limit=10'
```

## Response

```json
[
  {
    "type": "0x1::account::Account",
    "data": {
      "authentication_key": "0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f",
      "coin_register_events": {
        "counter": "0",
        "guid": {
          "id": {
            "addr": "0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f",
            "creation_num": "0"
          }
        }
      },
      "guid_creation_num": "2",
      "key_rotation_events": {
        "counter": "0",
        "guid": {
          "id": {
            "addr": "0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f",
            "creation_num": "1"
          }
        }
      },
      "rotation_capability_offer": {
        "for": { "vec": [] }
      },
      "sequence_number": "234285",
      "signer_capability_offer": {
        "for": { "vec": [] }
      }
    }
  }
]
```

## Response Definition

| Value | Data type | Description |
| --- | --- | --- |
| `type` | string | The type format of the account resource. |
| `data` | object | The additional data or information related to the account resource. |
| `authentication_key` | string | The authentication key used to verify the identity of the account owner. |
| `coin_register_events` | object | Events associated with coin registration for the account. |
| `counter` | string | The counter value associated with a particular operation. |
| `guid` | object | Unique identifier (GUID) of the resource. |
| `id` | object | The resource identifier. |
| `addr` | string | The address associated with the resource. |
| `creation_num` | string | The creation number of the resource. |
| `sequence_number` | string | Tracks the order of transactions initiated by the account. |

## Use Cases

This method can be used for:

* Checking on-chain state of tokens or smart contracts.
    
* Tracking smart contract event counters for deposits/withdrawals.
    
* Powering portfolio trackers or explorers that display account assets.
    
* Supporting conditional logic in dApps (e.g., only allow staking if a required resource exists).
    

## Code Examples

### Python (requests)

```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/resources?ledger_version=3357308041&limit=10"

response = requests.get(url)

print(response.text)
```

### Node.js (Axios)

```js
import axios from 'axios';

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/resources?ledger_version=3357308041&limit=10',
  headers: {}
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
```

> Replace `<ACCESS_TOKEN>` with your actual access token from GetBlock.

## Error Handling

* **Missing or invalid account hash** → Empty or malformed response.
    
* **403** → Access token is missing or invalid.
    

## Integration with Web3

By integrating `/v1/accounts/{account_hash}/resources`, developers can:

* Read smart contract resources tied to an account (for DeFi, staking, etc.).
    
* Power explorers or portfolio trackers to show account assets.
    
* Support reliable transaction pipelines and conditional dApp logic.
