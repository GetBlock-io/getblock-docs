---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Optimism

#### Parameters

`DATA` - string

Hash of a block.

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0x56436a935370b8decda78cf4a60e0668a882764af1cdec3a6d6967f944f4dace", false],
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
