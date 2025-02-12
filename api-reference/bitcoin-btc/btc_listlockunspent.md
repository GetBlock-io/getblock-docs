---
description: >-
  Example code for the listlockunspent  {disallowed} json-rpc method. Сomplete
  guide on how to use listlockunspent  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listlockunspent {disallowed} - Bitcoin

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://btc.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listlockunspent",
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
