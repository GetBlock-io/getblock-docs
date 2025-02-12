---
description: >-
  Example code for the getnetworkhashps json-rpc method. Сomplete guide on how
  to use getnetworkhashps json-rpc in GetBlock.io Web3 documentation.
---

# getnetworkhashps - Bitcoin Gold

#### Parameters

`nblocks` - numeric, optional, default=120

The number of blocks, or -1 for blocks since last difficulty change.

`height` - numeric, optional, default=-1

To estimate at the time of the given height.

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getnetworkhashps",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": 2324661.797680737
}
```
