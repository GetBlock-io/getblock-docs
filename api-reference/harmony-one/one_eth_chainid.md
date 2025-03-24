---
description: >-
  Example code for the eth_chainId json-rpc method. Сomplete guide on how to use
  eth_chainId json-rpc in GetBlock.io Web3 documentation.
---

# eth\_chainId - Harmony

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_chainId",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x63564c40"
}
```
