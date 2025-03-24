---
description: >-
  Example code for the setgenerate  {disallowed} json-rpc method. Сomplete guide
  on how to use setgenerate  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# setgenerate {disallowed} - Zcash

#### Parameters

`generate` - boolean

Set to true to turn on generation, off to turn off.

`genproclimit` - limit

Optional

Set the processor limit for when generation is on. Can be -1 for unlimited.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
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
