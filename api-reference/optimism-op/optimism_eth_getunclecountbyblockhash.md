---
description: >-
  Example code for the eth_getUncleCountByBlockHash json-rpc method. Сomplete
  guide on how to use eth_getUncleCountByBlockHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getUncleCountByBlockHash - Optimism

#### Parameters

`DATA` - string

hash of the block.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getUncleCountByBlockHash",
"params": ["0x56436a935370b8decda78cf4a60e0668a882764af1cdec3a6d6967f944f4dace"],
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
