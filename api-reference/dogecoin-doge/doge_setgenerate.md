---
description: >-
  Example code for the setgenerate  {disallowed} json-rpc method. Сomplete guide
  on how to use setgenerate  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# setgenerate {disallowed} - Dogecoin

#### Parameters

`generate` - boolean

is True or False to turn generation on or off.

`genproclimint` - integer

Number of processors that are used for generation, -1 is unlimited.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "setgenerate",
"params": [null, null],
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
