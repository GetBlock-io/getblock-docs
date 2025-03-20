---
description: >-
  Example code for the getFilterChanges  {disallowed} json-rpc method. Сomplete
  guide on how to use getFilterChanges  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getFilterChanges {disallowed} - Binance Smart Chain

#### Parameters

`QUANTITY` - None

The filter id

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getFilterChanges",
"params": [null],
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
