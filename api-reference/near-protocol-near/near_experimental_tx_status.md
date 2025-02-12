---
description: >-
  Example code for the EXPERIMENTAL_tx_status  {disallowed} json-rpc method.
  Сomplete guide on how to use EXPERIMENTAL_tx_status  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# EXPERIMENTAL\_tx\_status {disallowed} - NEAR Protocol

#### Parameters

`hash` - string

transaction hash

`id` - string

sender account id, used to determine which shard to query for transaction.

#### Request

```java
curl --location --request POST 'https://near.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "EXPERIMENTAL_tx_status",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
