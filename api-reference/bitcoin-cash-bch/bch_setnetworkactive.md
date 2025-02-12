---
description: >-
  Example code for the setnetworkactive  {disallowed} json-rpc method. Сomplete
  guide on how to use setnetworkactive  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# setnetworkactive {disallowed} - Bitcoin Cash

#### Parameters

`state` - boolean, required

true to enable networking, false to disable

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "setnetworkactive",
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
