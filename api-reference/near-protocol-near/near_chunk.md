---
description: >-
  Example code for the chunk json-rpc method. Сomplete guide on how to use chunk
  json-rpc in GetBlock.io Web3 documentation.
---

# chunk - NEAR Protocol

This method returns detailed information about a specific chunk. A chunk contains a subset of transactions, receipts, and state changes for a particular shard in the block.

## Supported Networks

- Mainnet

## Parameters

**Option 1**: Query based on a specific chunk ID.
| **Parameter**            | **Type** | **Required** | **Description**                                                 |
| ------------------------ | -------- | ------------ | --------------------------------------------------------------- |
| `chunk_id`            | string   | Yes        | The hash of the chunk you want to query.                  |

**Option 2**: Query based on block ID and shard ID.

| Parameter  | Type    | Required | Description                                                               |
|------------|---------|----------|---------------------------------------------------------------------------|
| `block_id` | integer | Yes      | The block identifier, which can be either a block number or a block hash. |
| `shard_id` | integer | yes      | The shard identifier.                                                     |

## Request

**Base URL**

```bash
  https://go.getblock.io/<ACCESS-TOKEN>/
```
**Example(cURL)**

```curl
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "chunk",
"params": {"block_id": 58934027, "shard_id": 0},
"id": "getblock.io"}'
```

## Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "author": "bitcat.pool.f863973.m0",
        "header": {
            "chunk_hash": "EBM2qg5cGr47EjMPtH88uvmXHDHqmWPzKaQadbWhdw22",
            "prev_block_hash": "2yUTTubrv1gJhTUVnHXh66JG3qxStBqySoN6wzRzgdVD",
            "outcome_root": "11111111111111111111111111111111",
            "prev_state_root": "HqWDq3f5HJuWnsTfwZS6jdAUqDjGFSTvjhb846vV27dx",
            "encoded_merkle_root": "9zYue7drR1rhfzEEoc4WUXzaYRnRNihvRoGt1BgK7Lkk",
            "encoded_length": 8,
            "height_created": 17821130,
            "height_included": 17821130,
            "shard_id": 0,
            "gas_used": 0,
            "gas_limit": 1000000000000000,
            "rent_paid": "0",
            "validator_reward": "0",
            "balance_burnt": "0",
            "outgoing_receipts_root": "H4Rd6SGeEBTbxkitsCdzfu9xL9HtZ2eHoPCQXUeZ6bW4",
            "tx_root": "11111111111111111111111111111111",
            "validator_proposals": [],
            "signature": "ed25519:4iPgpYAcPztAvnRHjfpegN37Rd8dTJKCjSd1gKAPLDaLcHUySJHjexMSSfC5iJVy28vqF9VB4psz13x2nt92cbR7"
        },
        "transactions": [],
        "receipts": []
    }
}
```
## Response Parameters Definition

| Field | Data Type | Description |
| :--- | :--- | :--- |
| `author` | string | The author name |
| `header` | object | The header of the chunk |
| `balance_burnt` | string | The number of tokens burnt (as a string to handle large numbers) |
| `bandwidth_requests` | object \| null | Information about bandwidth requests. If null, no bandwidth request data is available |
| `congestion_info` | object \| null | Details about network congestion. If null, no congestion data is available |
| `chunk_hash` | string | The hash of the chunk |
| `encoded_length` | integer | The chunk length encoded in integer format |
| `encoded_merkle_root` | string | The encoded merkle root |
| `gas_limit` | integer | The maximum gas allowed in this block |
| `gas_used` | integer | The total used gas by all transactions in this block |
| `height_created` | integer | The chunk height created |
| `height_included` | integer | The included height of the chunk |
| `outcome_root` | string | The root of the outcome trie of the block |
| `outgoing_receipts_root` | string | The root of the outgoing receipts of the block |
| `prev_block_hash` | string | The hash of the previous block chunk corresponding to the current one |
| `prev_state_root` | string | The previous state of the root of corresponding block |
| `rent_paid` | string | The number of tokens paid as rent (as a string) |
| `shard_id` | integer | The id of the multiple data grouped together |
| `signature` | string | The standard ed25519 signature type |
| `tx_root` | string | The transaction root |
| `validator_proposals` | array | An array of proposals provided by the validators |
| `validator_reward` | string | The reward sent to the validators (as a string) |
| `transactions` | array | An array of multiple transactions |
| `transactions.actions` | array | An action built from near-api-js.transactions |
| `transactions.actions.FunctionCall` | object | Represents a function call action within the transaction. |
| `transactions.actions.FunctionCall.args` | string | Base64-encoded arguments passed to the function. |
| `transactions.actions.FunctionCall.deposit` | string | The amount of tokens (in yocto) attached to the function call. |
| `transactions.actions.FunctionCall.gas` | integer | The amount of gas allocated for the function execution. |
| `transactions.actions.FunctionCall.method_name` | string | The name of the method being invoked in the function call. |
| `transactions.hash` | string | The hash of the transaction |
| `transactions.nonce` | integer | The number of transactions made by the sender prior to this one. |
| `transactions.priority_fee` | string | The priority fee (as a string) |
| `transactions.public_key` | string | The public key of the signer |
| `transactions.receiver_id` | string | The account id of the transaction receiver |
| `transactions.signature` | string | Standard ed25519 signature type |
| `transactions.signer_id` | string | The account id of the transaction originator |
| `receipts` | array | An array of receipts |

## Use Cases

  - Inspect individual chunks for analytics and auditing.
  - Get shard-specific transactions for parallel processing.
  - Analyse gas usage and validator rewards per chunk.
  - Monitor transaction outcomes at the shard level.

## Code Example

**Node(Axios)**
```js
import axios from "axios";
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "chunk",
  "params": {
    "chunk_id": "2A58YUxKi2yvWJBR5gtTtmBXN8ykoo48DZoYZRjgHYMF"
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
    "method": "chunk",
    "params": {
      "chunk_id": "2A58YUxKi2yvWJBR5gtTtmBXN8ykoo48DZoYZRjgHYMF"
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

| **HTTP Code**                 | **Error Message**                        | **Description**                                          |
| ----------------------------- | ---------------------------------------- | -------------------------------------------------------- |
| **400 Bad Request**           | `REQUEST_VALIDATION_ERROR`                       | The chunk hash or ID is malformed or missing.            |
| **422 Unprocessable Entity**             | `UNKNOWN_BLOCK`                        | The block hieght is missing or not correct                     |
| **500 Internal Server Error** | `Node error`                             | The NEAR node failed to process the request.             |
| **403 Forbidden**   | `RBAC:access denied` | The getblock access token is missing or incompelete |

## Integration with Web3

By integrating `chunk` into your dApp, developers can:

- Track shard-level activity for high-performance dApps.
- Validate chunk inclusion in blocks for cross-shard operations.
- Visualise gas consumption and transaction distribution across shards.
