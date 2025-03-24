---
description: >-
  Example code for the hmy_gasPrice json-rpc method. Сomplete guide on how to
  use hmy_gasPrice json-rpc in GetBlock.io Web3 documentation.
---

# hmy\_gasPrice - Harmony

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_gasPrice",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x174876e800"
}
```
