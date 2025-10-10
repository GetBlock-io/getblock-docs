---
description: >-
    Example code for the /v1/accounts/{account_hash}/resources JSON-RPC method. Сomplete guide on how to use /v1/accounts/{account_hash}/resources json-rpc in GetBlock.io Web3 documentation.
---

# /v1/accounts/{account_hash}/resources - Aptos

This endpoint gets **all resources** associated with a specific account at the latest ledger version. These resources include on-chain data such as the authentication key, sequence number, GUID, smart contract states, token etc. 

## Supported Networks

- Mainnet

## Parameter
<table>
  <tr>
   <td><strong>Parameter</strong>
   </td>
   <td><strong>Data type</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Required</strong>
   </td>
   <td><strong>In</strong>
   </td>
  </tr>
  <tr>
   <td><code>account_hash</code>
   </td>
   <td>string
   </td>
   <td>Aptos account address
   </td>
   <td>Yes
   </td>
   <td>Path
   </td>
  </tr>
  <tr>
   <td><code>ledger_version</code>
   </td>
   <td>string
   </td>
   <td>The ledger version to get the account state
   </td>
   <td>No
   </td>
   <td>query
   </td>
  </tr>
  <tr>
   <td><code>start</code>
   </td>
   <td>string
   </td>
   <td>The starting point for getting the resources
   </td>
   <td>No
   </td>
   <td>query
   </td>
  </tr>
  <tr>
   <td><code>limit</code>
   </td>
   <td>integer
   </td>
   <td>The maximum number of resources to retrieve per request
   </td>
   <td>No
   </td>
   <td>query
   </td>
  </tr>
</table>

## Request Example

**Base URL**

```
https://go.getblock.io/<ACCESS-TOKEN>/
```

**Example(cURL)**

```curl
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
                "for": {
                    "vec": []
                }
            },
            "sequence_number": "234285",
            "signer_capability_offer": {
                "for": {
                    "vec": []
                }
            }
        }
    }
]

```
## Response Definition

<table>
  <tr>
   <td>Value
   </td>
   <td>Data type
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td><code>type</code>
   </td>
   <td>string
   </td>
   <td>The type format of the account address
   </td>
  </tr>
  <tr>
   <td><code>data</code>
   </td>
   <td>object
   </td>
   <td>The additional data or information related to the account resource
   </td>
  </tr>
  <tr>
   <td><code>authentication_key</code>
   </td>
   <td>string
   </td>
   <td>The authentication key associated with the account to verify the identity of account owner
   </td>
  </tr>
  <tr>
   <td><code>coin_register_events</code>
   </td>
   <td>object
   </td>
   <td>The events associated with coin registration for the specified response
   </td>
  </tr>
  <tr>
   <td><code>counter</code>
   </td>
   <td>string
   </td>
   <td>The counter value associated with a particular operation
   </td>
  </tr>
  <tr>
   <td><code>guid</code>
   </td>
   <td>object
   </td>
   <td>the unique identifier (GUID) of the resource
   </td>
  </tr>
  <tr>
   <td><code>id</code>
   </td>
   <td>objecct
   </td>
   <td>The resource identifier
   </td>
  </tr>
  <tr>
   <td><code>addr</code>
   </td>
   <td>string
   </td>
   <td>The address associated with the resource
   </td>
  </tr>
  <tr>
   <td><code>creation_num</code>
   </td>
   <td>string
   </td>
   <td>The creation number of the resource
   </td>
  </tr>
  <tr>
   <td><code>sequence_number</code>
   </td>
   <td>string
   </td>
   <td>The sequence number tracks the order of transactions initiated by the account
   </td>
  </tr>
</table>

## Use Cases

This method can be used for:
* Checking on-chain state of tokens or smart contracts.
* Tracking smart contract event counters for deposits/withdrawals.

## Code Examples

**Python (requests)**

```python
import requests

url = "https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/resources?ledger_version=3357308041&limit=10"

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
```

**Node(Axios)**
```
import axios from 'axios';

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>/v1/accounts/0xbf9239be9eb7e7a3d8e4c1f36083464fd47e6bd1f82a43b7c0f7ee958705a52f/resources?ledger_version=3357308041&limit=10',
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
## Error Handling

| Status Code | Error Message   | Description / Cause                  |
|--------------|----------------|--------------------------------------|
| **410**      | `version_pruned` | The ledger version has been pruned   |
| **404**      | `version_not_found` | Ledger version not found             |


## **Integration with Web3**

By integrating `/v1/accounts/{account_hash}/resources`, developers can:

* Read smart contract resources tied to an account for dApps, such as DeFi or staking.

* Power explorers or portfolio trackers that display an account’s assets.

* Support conditional logic in dApps, e.g only allow a user to stake tokens if the resource exists in their account.
