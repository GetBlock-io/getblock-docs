---
description: >-
  Example code for the eth_newPendingTransactionFilter json-rpc method. Сomplete
  guide on how to use eth_newPendingTransactionFilter json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_newPendingTransactionFilter - Polygon

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
    "result": "0x5f1f04c3bd761f5c9f47d541cd4f5eec"
}
```
