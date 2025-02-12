---
description: >-
  Example code for the z_getnewaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use z_getnewaddress  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# z\_getnewaddress {disallowed} - Zcash

#### Parameters

`type` - string

Optional, default="sapling"

The type of address. One of \["sprout", "sapling"].

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "z_getnewaddress",
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
