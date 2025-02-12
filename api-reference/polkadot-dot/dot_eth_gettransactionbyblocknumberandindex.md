---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex  {disallowed}
  json-rpc method. Сomplete guide on how to use
  eth_getTransactionByBlockNumberAndIndex  {disallowed} json-rpc in GetBlock.io
  W
---

# eth\_getTransactionByBlockNumberAndIndex {disallowed} - Polkadot

#### Parameters

`number` - BlockNumber

None

`index` - U256

None

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
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
