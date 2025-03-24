---
description: >-
  Example code for the hmy_chainId json-rpc method. Сomplete guide on how to use
  hmy_chainId json-rpc in GetBlock.io Web3 documentation.
---

# hmy\_chainId - Harmony

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_chainId",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x1"
}
```
