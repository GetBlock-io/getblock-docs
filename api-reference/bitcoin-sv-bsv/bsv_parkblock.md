---
description: >-
  Example code for the parkblock  {disallowed} json-rpc method. Сomplete guide
  on how to use parkblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# parkblock {disallowed} - Bitcoin SV

#### Parameters

`blockhash` - string, required

the hash of the block to park

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "parkblock",
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
