---
description: >-
  Example code for the abortrescan  {disallowed} json-rpc method. Сomplete guide
  on how to use abortrescan  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# abortrescan {disallowed} - Bitcoin Cash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "abortrescan",
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
