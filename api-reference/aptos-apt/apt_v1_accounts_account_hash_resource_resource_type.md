---
description: >-
  Example code for the /v1/accounts/{account_hash}/resource/{resource_type}
  JSON-RPC method. Сomplete guide on how to use
  /v1/accounts/{account_hash}/resource/{resource_type} json-rpc in GetBlock.io
  Web
---

# /v1/accounts/{account\_hash}/resource/{resource\_type} - Aptos

This endpoint gets an individual resource from a given account and at a specific ledger version. This is more specific than /resources since it targets one resource type directly.

## Supported Networks

* Mainnet

## Parameters

| **Parameter**    | **Data type** | **Description**                                    | **Required** | **In** |
| ---------------- | ------------- | -------------------------------------------------- | ------------ | ------ |
| `account_hash`   | string        | Aptos account address                              | Yes          | Path   |
| `resource_type`  | string        | The type format of the account address to retrieve | Yes          | path   |
| `ledger_version` | string        | The ledger version to get the account state        | No           | query  |

## Request Example

**Base URL**

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Example(cURL)**

```curl
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/resource/0x1::account::Account'
```

## Response

```json
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
            "for": {
                "vec": []
            }
        },
        "sequence_number": "250163",
        "signer_capability_offer": {
            "for": {
                "vec": []
            }
        }
    }
}

```

## Response Parameter Definition

| Value                  | Data type | Description                                                                                |
| ---------------------- | --------- | ------------------------------------------------------------------------------------------ |
| type                   | string    | The type format of the account address                                                     |
| data                   | object    | The additional data or information related to the account resource                         |
| authentication\_key    | string    | The authentication key associated with the account to verify the identity of account owner |
| coin\_register\_events | object    | The events associated with coin registration for the specified response                    |
| counter                | string    | The counter value associated with a particular operation                                   |
| guid                   | object    | the unique identifier (GUID) of the resource                                               |
| id                     | objecct   | The resource identifier                                                                    |
| addr                   | string    | The address associated with the resource                                                   |
| creation\_num          | string    | The creation number of the resource                                                        |
| sequence\_number       | string    | The sequence number tracks the order of transactions initiated by the account              |

## Use Cases

This method can be used to:

* Fetch a specific token balance without retrieving all account resources.
* Query a single resource type (like a staking pool or NFT ownership).
* Used in wallets and DeFi apps where targeted resource data is required.

## Code Examples

**Python (Requests)**

```python
import requests

url = "https://go.getblock.io/b293629b1b3a4e188efb3c4e94d133b6/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/resource/0x1::account::Account"
response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)

```

**Node(Axios)**

```js
import axios from 'axios'

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/b293629b1b3a4e188efb3c4e94d133b6/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/resource/0x1::account::Account',
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});


```

## Error handling

The possible error you may experience includes the following:

| **Status Code** | **Error Message**     | **Cause**                                                |
| --------------- | --------------------- | -------------------------------------------------------- |
| 403             | Forbidden             | Missing or invalid ACCESS\_TOKEN.                        |
| 404             | Resource not found    | The given resource type does not exist for this account. |
| 500             | Internal server error | Node or network issue. Retry later.                      |

## Integration with Web3

By integrating `/v1/accounts/{account_hash}/resource/{resource_type`, developers can:

* Query a single CoinStore to show token balances in wallets.
* Validate a user’s participation in staking, liquidity pools, or governance.
* Reduce bandwidth by fetching only the resource you need, instead of all resources.
* Pull targeted NFT or token data for user profiles or marketplaces.
