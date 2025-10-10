---
description: >-
    Example code for the /v1/accounts/{account_hash}/resource/{resource_type} JSON-RPC method. Сomplete guide on how to use /v1/accounts/{account_hash}/resource/{resource_type} json-rpc in GetBlock.io Web3 documentation.
---

# /v1/accounts/{account_hash}/resource/{resource_type} - Aptos

This endpoint gets an individual resource from a given account and at a specific ledger version.  This is more specific than /resources since it targets one resource type directly.


## Supported Networks

* Mainnet

## Parameters


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
   <td><code>resource_type</code>
   </td>
   <td>string
   </td>
   <td>The type format of the account address to retrieve
   </td>
   <td>Yes
   </td>
   <td>path
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
</table>



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
   <td>type
   </td>
   <td>string
   </td>
   <td>The type format of the account address
   </td>
  </tr>
  <tr>
   <td>data
   </td>
   <td>object
   </td>
   <td>The additional data or information related to the account resource
   </td>
  </tr>
  <tr>
   <td>authentication_key
   </td>
   <td>string
   </td>
   <td>The authentication key associated with the account to verify the identity of account owner
   </td>
  </tr>
  <tr>
   <td>coin_register_events
   </td>
   <td>object
   </td>
   <td>The events associated with coin registration for the specified response
   </td>
  </tr>
  <tr>
   <td>counter
   </td>
   <td>string
   </td>
   <td>The counter value associated with a particular operation
   </td>
  </tr>
  <tr>
   <td>guid
   </td>
   <td>object
   </td>
   <td>the unique identifier (GUID) of the resource
   </td>
  </tr>
  <tr>
   <td>id
   </td>
   <td>objecct
   </td>
   <td>The resource identifier
   </td>
  </tr>
  <tr>
   <td>addr
   </td>
   <td>string
   </td>
   <td>The address associated with the resource
   </td>
  </tr>
  <tr>
   <td>creation_num
   </td>
   <td>string
   </td>
   <td>The creation number of the resource
   </td>
  </tr>
  <tr>
   <td>sequence_number
   </td>
   <td>string
   </td>
   <td>The sequence number tracks the order of transactions initiated by the account
   </td>
  </tr>
</table>



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

<table>
  <tr>
   <td><strong>Status Code</strong>
   </td>
   <td><strong>Error Message</strong>
   </td>
   <td><strong>Cause</strong>
   </td>
  </tr>
  <tr>
   <td>403
   </td>
   <td>Forbidden
   </td>
   <td>Missing or invalid ACCESS_TOKEN.
   </td>
  </tr>
  <tr>
   <td>404
   </td>
   <td>Resource not found
   </td>
   <td>The given resource type does not exist for this account.
   </td>
  </tr>
  <tr>
   <td>500
   </td>
   <td>Internal server error
   </td>
   <td>Node or network issue. Retry later.
   </td>
  </tr>
</table>

## Integration with Web3

By integrating `/v1/accounts/{account_hash}/resource/{resource_type`, developers can:

* Query a single CoinStore to show token balances in wallets.
* Validate a user’s participation in staking, liquidity pools, or governance.
* Reduce bandwidth by fetching only the resource you need, instead of all resources.
* Pull targeted NFT or token data for user profiles or marketplaces.
