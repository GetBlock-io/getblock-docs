---
description: >-
  Example code for the getblockcount json-rpc method. Сomplete guide on how to
  use getblockcount json-rpc in GetBlock.io Web3 documentation.
---

# getblockcount - Bitcoin Gold

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockcount",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": 702634
}
```
