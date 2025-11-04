---
description: >-
  Example code for the query(view_code) json-rpc method. Сomplete guide on how
  to use query(view_code) json-rpc in GetBlock.io Web3 documentation.
---

# query(view\_code) - NEAR Protocol

This method returns the **smart contract code** deployed on a specific NEAR account. It provides the **WASM bytecode** and metadata about the code hash, enabling developers to verify and inspect deployed contracts.

## Supported Networks

- Mainnet

## Parameters Description

| **Parameter**    | **Type** | **Required** | **Description**                                                        |
| ---------------- | -------- | ------------ | ---------------------------------------------------------------------- |
| `request_type` | string   | Yes        | Must be set to `"view_code"`.                                          |
| `finality`     | string   | Yes     | Defines how finalised the data should be: `"final"`, `"near-final"` or `"optimistic"`. |
| `account_id`  | string   | Yes        | The NEAR account ID of the contract to inspect.                        |

## Request

**Base URL**

```bash
 https://go.getblock.io/<ACCESS_TOKEN>
```
**Example(cURL)**

```bash
curl --location 'https://go.getblock.io/<ACCESS_TOKEN>' \
--header 'Content-Type: application/json' \
--data '{"jsonrpc": "2.0",
"method": "query",
"params": {"request_type": "view_code", "finality": "near-final", "account_id": "staked.poolv1.near"},
"id": "getblock.io"}'
```

## Response Example

```json
{
    "jsonrpc": "2.0",
    "result": {
        "block_hash": "5xyv8AMZAknNgSZUug6uqKaTt2TX5hY3zxoEWLDD3mSV",
        "block_height": 169729125,
        "code_base64": "AGFzbQEAAAAB7wNBYAJ/fwF/YAF...",
        "hash": "J1arLz48fgXcGyCPVckFwLnewNH6j1uw79thsvwqGYTY"
    },
    "id": "getblock.io"
}
```

## Response Parameters Definition

| **Field**        | **Type** | **Description**                                                      |
| ---------------- | -------- | -------------------------------------------------------------------- |
| **block_hash**   | string   | The hash of the block that contains the account state.               |
| **block_height** | integer  | The height of the block at which the data was retrieved.             |
| **code_base64**  | string   | The base64-encoded WebAssembly (WASM) code of the smart contract.    |
| **hash**         | string   | The SHA-256 hash of the contract’s WASM code, used for verification. |


## Use Cases
- Verify a contract’s **deployed bytecode** for audit or testing.
- Fetch **WASM code** for on-chain contract analysis tools.
- Ensure deployed code integrity for **security monitoring**.

## Code Example

**Node(Axios)**
```js
import axios from "axios";
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "query",
  "params": {
    "request_type": "view_code",
    "finality": "near-final",
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
    "request_type": "view_code",
    "finality": "near-final",
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

| **HTTP Code**                 | **Error Message**              | **Description**                                       |
| ----------------------------- | ------------------------------ | ----------------------------------------------------- |
| **404 Not Found**             | `Contract not found`           | The specified account has no deployed smart contract. |
| **500 Internal Server Error** | `Node processing error`        | The NEAR node failed to process the query.            |
| **403 Forbidden**   | `RBAC: access denied` | The Getblock access token is missing or incorrect    |


## Integration with Web3

By integrating `/query (view_code)` into dApp, developers can:

- Enable **smart contract inspection** directly from dashboards.
- Build **code verification tools** or explorers.
- Integrate with **CI/CD pipelines** for contract deployment validation.
- Enhance transparency for **on-chain AI and dApp ecosystems**.
