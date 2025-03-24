---
description: >-
  Example code for the eth_getWork json-rpc method. Сomplete guide on how to use
  eth_getWork json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getWork - Ethereum Classic

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getWork",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        "0xce5e32ca59cb86799a1879e90150b2c3b882852173e59865e9e79abb67a9d636",
        "0x0000000000000000000000000000000000000000000000000000000000000000",
        "0x00a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3d70a3",
        "0x42"
    ]
}
```
