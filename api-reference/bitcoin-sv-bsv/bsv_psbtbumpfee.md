---
description: >-
  Example code for the psbtbumpfee  {disallowed} json-rpc method. Сomplete guide
  on how to use psbtbumpfee  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# psbtbumpfee {disallowed} - Bitcoin SV

#### Parameters

`txid` - string, required

The txid to be bumped

`options` - json object, optional

None

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "psbtbumpfee",
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
