---
description: >-
  Example code for the hmy_newPendingTransactionFilter json-rpc method. Сomplete
  guide on how to use hmy_newPendingTransactionFilter json-rpc in GetBlock.io
  Web3 documentation.
---

# hmy\_newPendingTransactionFilter - Harmony

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_newPendingTransactionFilter",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x4d09fd774f646f97b08c50662d1ddb1f"
}
```
