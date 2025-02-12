---
description: >-
  Example code for the getaccount  {disallowed} json-rpc method. Сomplete guide
  on how to use getaccount  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getaccount {disallowed} - Zcash

#### Parameters

`zcashaddress` - string

The Zcash address for account lookup.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getaccount",
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
