---
description: >-
  Example code for the z_listreceivedbyaddress  {disallowed} json-rpc method.
  Сomplete guide on how to use z_listreceivedbyaddress  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# z\_listreceivedbyaddress {disallowed} - Zcash

#### Parameters

`address` - string

The private address

`minconf` - numeric

Optional, default=1

Only include transactions confirmed at least this many times.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "z_listreceivedbyaddress",
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
