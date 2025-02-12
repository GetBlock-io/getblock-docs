---
description: >-
  Example code for the getaccountaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use getaccountaddress  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getaccountaddress {disallowed} - Dogecoin

#### Parameters

`account` - string

Account for which the address should be returned.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getaccountaddress",
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
