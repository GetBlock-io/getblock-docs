---
description: >-
  Example code for the eth_getUncleCountByBlockNumber  {disallowed} json-rpc
  method. Сomplete guide on how to use eth_getUncleCountByBlockNumber 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getUncleCountByBlockNumber {disallowed} - Polkadot

#### Parameters

`number` - BlockNumber

None

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getUncleCountByBlockNumber",
"params": [null],
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
