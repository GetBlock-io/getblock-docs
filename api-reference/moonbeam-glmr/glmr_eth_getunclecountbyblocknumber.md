---
description: >-
  Example code for the eth_getUncleCountByBlockNumber json-rpc method. Сomplete
  guide on how to use eth_getUncleCountByBlockNumber json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getUncleCountByBlockNumber - Moonbeam

#### Parameters

`QUANTITY|TAG` - string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getUncleCountByBlockNumber",
"params": ["0xc6f437"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x0"
}
```
