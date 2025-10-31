---
description: >-
  Example code for the query(view_account) json-rpc method. Сomplete guide on
  how to use query(view_account) json-rpc in GetBlock.io Web3 documentation.
---

# query(view\_account) - NEAR Protocol

This method get **on-chain details** of a NEAR account, such as its **balance**, **locked tokens**, **storage usage**, and **account code hash**.

## Supported Networks

- Mainnet

## Parameters 

| **Parameter**    | **Type** | **Required** | **Description**                                                                                     |
| ---------------- | -------- | ------------ | --------------------------------------------------------------------------------------------------- |
| `request_type` | string   | Yes        | Must be set to `"view_account"`.                                                                    |
| `finality`     | string   | Yes     | Determines how final the queried data is. Options: `"optimistic"`, `"final"`. Default is `"final"`. |
| `account_id`  | string   | Yes        | NEAR account ID to retrieve information for.  |

## Request Example

**Base URL**

```bash
https://go.getblock.io/<ACCESS_TOKEN>
```

**Example(cURL)**

```curl
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{"jsonrpc": "2.0",
"method": "query",
"params": {"request_type": "view_account", "finality": "final", "account_id": "staked.poolv1.near", "method_name": "get_num"},
"id": "getblock.io"}'
```

## Response

```json
{
    "jsonrpc": "2.0",
    "result": {
        "amount": "51268499170950517816171605957",
        "block_hash": "2vhW5kHxFMeESzzoVxtNonbYr6NYGrjXdzoZcvG9Hd2t",
        "block_height": 169724949,
        "code_hash": "J1arLz48fgXcGyCPVckFwLnewNH6j1uw79thsvwqGYTY",
        "locked": "1877029115796304585995032900305",
        "storage_paid_at": 0,
        "storage_usage": 1270683
    },
    "id": "getblock.io"
}
```
## Response Parameters

| **Field**           | **Type** | **Description**                                                       |
| ------------------- | -------- | --------------------------------------------------------------------- |
| **amount**          | string   | The available balance of the account in yoctoNEAR.                    |
| **locked**          | string   | Amount of tokens locked due to staking.                               |
| **code_hash**       | string   | Hash of the smart contract code associated with the account (if any). |
| **storage_usage**   | integer  | Amount of storage (in bytes) used by the account.                     |
| **storage_paid_at** | integer  | Block height when storage was last paid.                              |
| **block_height**    | integer  | The height of the block when the account information was fetched.     |
| **block_hash**      | string   | Hash of the block used to query the account data.                     |

## Use Cases

* Retrieve account balance and locked tokens for wallets or dApps.
* Verify if an account has a deployed contract (via `code_hash`).
* Monitor storage usage and costs for on-chain applications.
* Validate account state before performing transfers or contract calls.

## Code Example

**Node(Axios)**
  ```js
  import axios from "axios";
  let data = JSON.stringify({
    "jsonrpc": "2.0",
    "method": "query",
    "params": {
      "request_type": "view_account",
      "finality": "final",
      "account_id": "staked.poolv1.near",
      "method_name": "get_num"
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
      "request_type": "view_account",
      "finality": "final",
      "account_id": "staked.poolv1.near",
      "method_name": "get_num"
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

| **HTTP Code**                 | **Error Message**              | **Description**                                            |
| ----------------------------- | ------------------------------ | ---------------------------------------------------------- |
| **404 Not Found**             | `HANDLER_ERROR`            | The specified account does not exist on the network or is incorrect      |
| **500 Internal Server Error** | `Node error`                   | Node failed to process the query or returned invalid data or network issues |
| **403 Service Forbidden**   | `RBAC: access denied` | The Getblock access token is missing or not correct             |

## Integration with Web3

By integrating `query (view_account)` into dApp, developers can:

- Build **wallets** and **portfolio trackers** showing real-time balances.
- Pre-validate **account existence** before sending transactions.
- Fetch **read-only on-chain data** without consuming gas.
