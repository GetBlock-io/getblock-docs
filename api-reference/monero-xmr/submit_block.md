---
description: >-
  Example code for the submit_block json-rpc method. Сomplete guide on how to
  use submit_block json-rpc in GetBlock.io Web3 documentation.
---

# submit\_block - Monero

#### Parameters

`Block blob data` - array of strings

list of block blobs which have been mined.

#### Request

```java
curl --location --request POST 'https://xmr.getblock.io/mainnet/json_rpc' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "submit_block",
"params": {"Block blob data": "0707e6bdfedc053771512f1bc27c62731ae9e8f2443db64ce742f4e57f5cf8d393de28551e441a0000000002fb830a01ffbf830a018cfe88bee283060274c0aae2ef5730e680308d9c00b6da59187ad0352efe3c71d36eeeb28782f29f2501bd56b952c3ddc3e350c2631d3a5086cac172c56893831228b17de296ff4669de020200000000"},
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -7,
        "message": "Block not accepted"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
