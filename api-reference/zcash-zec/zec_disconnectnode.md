---
description: >-
  Example code for the disconnectnode  {disallowed} json-rpc method. Сomplete
  guide on how to use disconnectnode  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# disconnectnode {disallowed} - Zcash

#### Parameters

`node` - string

The node.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "disconnectnode",
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
