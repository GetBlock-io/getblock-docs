---
description: >-
  Example code for the query(view_account_basic) json-rpc method. Сomplete guide
  on how to use query(view_account_basic) json-rpc in GetBlock.io Web3
  documentation.
---

# query(view\_account\_basic) - NEAR Protocol

The method provides **lightweight account information** such as **balance**, **locked tokens**, and **storage usage**. Unlike `view_account`, this version returns **only essential fields**, making it **faster and more efficient** for applications that do not require full account metadata.

## Supported Networks

- Mainnet

## Parameters

| **Parameter**    | **Type** | **Required** | **Description**                                                                                     |
| ---------------- | -------- | ------------ | --------------------------------------------------------------------------------------------------- |
| `request_type` | string   | Yes        | Must be set to `"view_account_basic"`.                                                              |
| `finality`    | string   | Yes     | Determines how final the queried data is. Options: `"optimistic"`, `"near-final"`, `"final"`. Default is `"final"`. |
| **account_id**   | string   | Yes        | The NEAR account ID whose data you want to fetch.                                                   |
## Request

**Base URL**

```bash
 https://go.getblock.io/<ACCESS_TOKEN>
```
**Example(cURL)**

```curl
curl -X POST https://go.getblock.io/<ACCESS_TOKEN> \
-H "Content-Type: application/json" \
-d '{
  "jsonrpc": "2.0",
  "id": "view-account-basic",
  "method": "query",
  "params": {
    "request_type": "view_account_basic",
    "finality": "final",
    "account_id": "example-account.near"
  }
}'
```

## Response Example

```json
{
    "jsonrpc": "2.0",
    "result": {
        "amount": "51268499170950517816171605957",
        "block_hash": "8YFaUhxEhBdBusPsuZHru3eRBtH99GHHtz8KoPRXAAXR",
        "block_height": 169727069,
        "code_hash": "J1arLz48fgXcGyCPVckFwLnewNH6j1uw79thsvwqGYTY",
        "locked": "1877029115796304585995032900305",
        "storage_paid_at": 0,
        "storage_usage": 1270683
    },
    "id": "getblock.io"
}
```

## Response Parameters Definition

| **Field**           | **Type** | **Description**                                                       |
| ------------------- | -------- | --------------------------------------------------------------------- |
| **amount**          | string   | Available account balance in yoctoNEAR (1 yoctoNEAR = 10⁻²⁴ NEAR).    |
| **block_hash**      | string   | The hash of the block from which the data was retrieved.              |
| **block_height**    | integer  | The height of the block at which the data was retrieved.              |
| **code_hash**       | string   | The hash of the smart contract code deployed on the account (if any). |
| **locked**          | string   | Amount of tokens locked for staking.                                  |
| **storage_paid_at** | integer  | Block height when the account last paid for storage.                  |
| **storage_usage**   | integer  | The number of bytes used by this account on-chain.                    |


## Use Cases

* Fetch simplified account balances for wallets or analytics dashboards.
* Display balance information without full contract or storage metadata.
* Improve performance for applications requiring **quick balance checks**.
* Monitor changes in token balances across multiple accounts.

## Code Example

**Node(Axios)**

```js
import axios from 'axios';
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "query",
  "params": {
    "request_type": "view_account",
    "finality": "final",
    "account_id": "staked.poolv1.near"
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
    "account_id": "staked.poolv1.near"
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

| **HTTP Code**                 | **Error Message**              | **Description**                                     |
| ----------------------------- | ------------------------------ | --------------------------------------------------- |
| **404 Not Found**             | `Account not found`            | The account does not exist on the NEAR network or is incorrect    |
| **500 Internal Server Error** | `Node processing error`        | NEAR node failed to process or return account data. |
| **403 Forbidden**   | `RBAC: access denied` | The getblock access token is missing or incorrect |

## Integration with Web3

By integrating `query(view_account_basic)` into dApp, developers can:

* Power **real-time wallet balance tracking**.
* Optimise data fetching for **batch account queries**.
* Reduce latency in lightweight front-end applications.
* Combine with `/gas_price` and `/view_account` for full transaction insights.
