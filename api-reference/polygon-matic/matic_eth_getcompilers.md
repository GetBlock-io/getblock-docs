---
description: >-
  Example code for the eth_getCompilers  {disallowed} json-rpc method. Сomplete
  guide on how to use eth_getCompilers  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getCompilers {disallowed} - Polygon

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getCompilers",
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
