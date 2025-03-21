---
description: >-
  Example code for the combinerawtransaction json-rpc method. Сomplete guide on
  how to use combinerawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# combinerawtransaction - Bitcoin Cash

#### Parameters

`txs` - json array, required

A json array of hex strings of partially signed transactions

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "combinerawtransaction",
"params": [["myhex1", "myhex2", "myhex3"]],
"id": "getblock.io"}'
```

#### Response

```java
null
```
