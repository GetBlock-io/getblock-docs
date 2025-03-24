---
description: >-
  Example code for the eth_newPendingTransactionFilter json-rpc method. Сomplete
  guide on how to use eth_newPendingTransactionFilter json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_newPendingTransactionFilter - Fuse Network

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_newPendingTransactionFilter",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xe"
}
```
