---
description: >-
  Example code for the putString  {disallowed} json-rpc method. Сomplete guide
  on how to use putString  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# putString {disallowed} - Binance Smart Chain

#### Parameters

`String` - None

Database name

`String` - None

Key name

`String` - None

String to store

#### Request

```java
curl --curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "putString",
"params": [null, null, null],
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
