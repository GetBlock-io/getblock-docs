---
description: >-
  Example code for the system_syncState json-rpc method. Сomplete guide on how
  to use system_syncState json-rpc in GetBlock.io Web3 documentation.
---

# system\_syncState - Kusama

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "system_syncState",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "currentBlock": 16049622,
        "highestBlock": 16049622,
        "startingBlock": 16032618
    }
}
```
