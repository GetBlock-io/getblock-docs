---
description: >-
  Example code for the getMessages  {disallowed} json-rpc method. Сomplete guide
  on how to use getMessages  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getMessages {disallowed} - Binance Smart Chain

#### Parameters

`QUANTITY` - None

The filter id

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getMessages",
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
