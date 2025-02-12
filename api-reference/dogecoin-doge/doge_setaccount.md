---
description: >-
  Example code for the setaccount  {disallowed} json-rpc method. Сomplete guide
  on how to use setaccount  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# setaccount {disallowed} - Dogecoin

#### Parameters

`dogecoinaddress` - string

Dogecoin address to associate.

`account` - string

Account to associate the address to.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "setaccount",
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
