---
description: >-
  Example code for the hmyv2_blockNumber json-rpc method. Сomplete guide on how
  to use hmyv2_blockNumber json-rpc in GetBlock.io Web3 documentation.
---

# hmyv2\_blockNumber - Harmony

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "hmyv2_blockNumber",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": 42466343
}
```
