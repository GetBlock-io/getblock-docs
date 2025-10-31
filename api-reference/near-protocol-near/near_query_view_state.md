---
description: >-
  Example code for the query(view_state) json-rpc method. Сomplete guide on how
  to use query(view_state) json-rpc in GetBlock.io Web3 documentation.
---

# query(view\_state) - NEAR Protocol

This method returns the **key-value pairs** stored in a smart contract’s on-chain state.
It allows developers to **read contract data**, **inspect stored variables**, or **analyse application states** directly from the NEAR blockchain.


## Supported Networks

 - Mainnet
 
 ## Parameters 

| **Parameter**     | **Type** | **Required** | **Description**                                                                       |
| ----------------- | -------- | ------------ | ------------------------------------------------------------------------------------- |
| `request_type`  | string   | Yes        | Must be set to `"view_state"`.                                                        |
| `finality`      | string   | Yes     | Defines data consistency: `"final"`, `"near-final"` or `"optimistic"`. Default is `"final"`.          |
| `account_id`   | string   | Yes        | The NEAR account ID that owns the contract.                                           |
| `prefix_base64` | string   | Optional     | Base64-encoded key prefix filter. Returns only matching keys (use `""` to fetch all). |

## Request

**Base URL**

```bash
https://go.getblock.io/<ACCESS_TOKEN>
```
**Example(cURL)**

```bash
curl -X POST https://go.getblock.io/<ACCESS_TOKEN>\
-H "Content-Type: application/json" \
-d '{"jsonrpc": "2.0",
"method": "query",
"params": {
    "request_type": "view_state", 
    "finality": "final", 
    "account_id": "kiln.poolv1.near",
    "prefix_base64": ""
    },
"id": "getblock.io"}
'
```

## Response Example

```json
{
    "jsonrpc": "2.0",
    "result": {
        "block_hash": "C3usKbGCgW2AKtniikNhGMPCvewEJVgQZuWG8BxFa6kV",
        "block_height": 169786589,
        "values": [
            {
                "key": "U1RBVEU=",
                "value": "EwAAAGtpbG5fdmFsaWRhdG9yLm5lYXIhAAAAAOFYNYzJjjco+fLi0fJgla9B4s+AMVVdTepWc9pwE5wYdg4AAAAAAABQ4axFY8Ct82YE5ctMAAAAZzmi/dlPKeusHd/LTAAAAGc5ov3ZTynrrB3fy0wAAABkAAAAZAAAAAIAAAB1aU0AAAAAAAAAAgAAAHVrTQAAAAAAAAACAAAAdXYA"
            },
            {
                "key": "dWkJAAAAc2VwaC5uZWFy",
                "value": "EgAAAAAAAAA="
            },
            {
                "key": "dWkKAAAAZGRzaW4ubmVhcg==",
                "value": "DQAAAAAAAAA="
            },
            {
                "key": "dWkLAAAAc210X2Jvc3MudGc=",
                "value": "JQAAAAAAAAA="
            },
            {
                "key": "dWlAAAAAZWQwNWI3ZmJiMmZkZDRhNWFhYmRmYjk5YTkxYTVjYzQzNjc3ZGE2MmU3NGFkOGUyMTI4MjAxYTNjYjVhMGRlYQ==",
                "value": "EwAAAAAAAAA="
            }
        ]
    },
    "id": "getblock.io"
}
```

## Response Parameters Definition

| **Field**        | **Type** | **Description**                                         |
| ---------------- | -------- | ------------------------------------------------------- |
| **block_height** | integer  | Height of the block from which the state was retrieved. |
| **block_hash**   | string   | Hash of the block containing the contract state.        |
| **values**       | array    | List of key-value pairs stored in the contract’s state. |
| **values.key**   | string   | Base64-encoded storage key.                             |
| **values.value** | string   | Base64-encoded value associated with the key.           |


## Use Cases

- Get **contract-level configuration** for analytics or dashboards.
- Debug **smart contract storage** during development or audits.
- Build **indexers** or off-chain data sync tools.

## Code Example

**Node(Axios)**
```js
import axios from 'axios';
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "query",
  "params": {
    "request_type": "view_state",
    "finality": "final",
    "account_id": "kiln.poolv1.nea",
    "prefix_base64": ""
  },
  "id": "getblock.io"
});

let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>',
  headers: { 
    'Content-Type': 'application/json'
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
**Python(Requests)**
  ```python
  import requests
  import json
  
  url = "https://go.getblock.io/<ACCESS_TOKEN>"
  
  payload = json.dumps({
    "jsonrpc": "2.0",
    "method": "query",
    "params": {
      "request_type": "view_state",
      "finality": "final",
      "account_id": "kiln.poolv1.nea",
      "prefix_base64": ""
    },
    "id": "getblock.io"
  })
  headers = {
    'Content-Type': 'application/json'
  }
  
  response = requests.request("POST", url, headers=headers, data=payload)
  
  print(response.text)
  
   ```
## Error Handling

| **HTTP Code**                 | **Error Message**              | **Description**                                      |
| ----------------------------- | ------------------------------ | ---------------------------------------------------- |
| **400 Bad Request**           | `UNKNOWN_ACCOUNT`           | The account ID format is incorrect.                  |
| **500 Internal Server Error** | `Node processing error`        | The NEAR node encountered an issue retrieving state. |
| **403 Forbidden**   | `RBAC: access denied` | The Getblock access token is missing or incorrect                |


## Integration with Web3

By integrating `query(view_state)` into dApp, developers can:

* Read **contract data** directly without calling view functions.
* Build **state inspection tools** and dApp dashboards.
* Automate **off-chain synchronization** for analytics and monitoring.
* Create **AI agents** that interpret blockchain data dynamically.
