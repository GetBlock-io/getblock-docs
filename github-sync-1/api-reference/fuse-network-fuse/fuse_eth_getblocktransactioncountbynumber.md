---
description: >-
  Example code for the eth_getBlockTransactionCountByNumber json-rpc method.
  Сomplete guide on how to use eth_getBlockTransactionCountByNumber json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getBlockTransactionCountByNumber - Fuse Network

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockTransactionCountByNumber",
"params": ["latest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x2"
}
```
