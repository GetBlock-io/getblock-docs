---
description: >-
  Example code for the rollup_getInfo json-rpc method. Сomplete guide on how to
  use rollup_getInfo json-rpc in GetBlock.io Web3 documentation.
---

# rollup\_getInfo - Optimism

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "rollup_getInfo",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "ethContext": {
            "blockNumber": 17376329,
            "timestamp": 1685506009
        },
        "mode": "verifier",
        "rollupContext": {
            "index": 103110437,
            "queueIndex": 354301,
            "verifiedIndex": 0
        },
        "syncing": false
    }
}
```
