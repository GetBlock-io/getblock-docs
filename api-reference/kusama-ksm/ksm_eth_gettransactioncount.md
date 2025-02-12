---
description: >-
  Example code for the eth_getTransactionCount  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_getTransactionCount  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getTransactionCount {disallowed} - Kusama

#### Parameters

`hash` - H256

None

`number` - BlockNumber

None

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionCount",
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
