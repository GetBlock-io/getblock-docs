---
description: >-
  Example code for the getmininginfo json-rpc method. Сomplete guide on how to
  use getmininginfo json-rpc in GetBlock.io Web3 documentation.
---

# getmininginfo - Zcash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getmininginfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "blocks": 1384382,
        "currentblocksize": 0,
        "currentblocktx": 0,
        "difficulty": 49663639.85134914,
        "errors": "",
        "errorstimestamp": 1631193056,
        "genproclimit": 1,
        "localsolps": 0,
        "networksolps": 5297326416,
        "networkhashps": 5297326416,
        "pooledtx": 4,
        "testnet": false,
        "chain": "main",
        "generate": false
    }
}
```
