---
description: >-
  Example code for the getblocknumber  {disallowed} json-rpc method. Сomplete
  guide on how to use getblocknumber  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getblocknumber {disallowed} - Dogecoin

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblocknumber",
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
