---
description: >-
  Example code for the eth_newPendingTransactionFilter  {disallowed} json-rpc
  method. Сomplete guide on how to use eth_newPendingTransactionFilter 
  {disallowed} json-rpc in GetBlock.io Web3 documentatio
---

# eth\_newPendingTransactionFilter {disallowed} - Moonriver

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
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
