---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex  {disallowed}
  json-rpc method. Сomplete guide on how to use
  eth_getTransactionByBlockNumberAndIndex  {disallowed} json-rpc in GetBlock.io
  W
---

# eth\_getTransactionByBlockNumberAndIndex {disallowed} - Kusama

#### Parameters

`number` - BlockNumber

None

`index` - U256

None

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
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
