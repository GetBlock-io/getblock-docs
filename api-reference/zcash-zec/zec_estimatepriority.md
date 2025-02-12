---
description: >-
  Example code for the estimatepriority json-rpc method. Сomplete guide on how
  to use estimatepriority json-rpc in GetBlock.io Web3 documentation.
---

# estimatepriority - Zcash

#### Parameters

`nblocks` - numeric

None

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "estimatepriority",
"params": [5],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": -1
}
```
