---
description: >-
  Example code for the eth_getUncleCountByBlockHash json-rpc method. Сomplete
  guide on how to use eth_getUncleCountByBlockHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getUncleCountByBlockHash - Binance Smart Chain

#### Parameters

`DATA` - None

32-byte block hash.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getUncleCountByBlockHash",
"params": ["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": null
}
```
