---
description: >-
  Example code for the addToGroup  {disallowed} json-rpc method. Сomplete guide
  on how to use addToGroup  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# addToGroup {disallowed} - Binance Smart Chain

#### Parameters

`DATA` - None

60 Bytes - The identity address to add to a group

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "addToGroup",
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
