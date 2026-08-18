---
description: >-
  Example code for the gettxoutsetinfo  {disallowed} json-rpc method. Сomplete
  guide on how to use gettxoutsetinfo  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# gettxoutsetinfo {disallowed} - DigiByte

#### Parameters

`hash_type` - string, optional, default=hash\_serialized\_2

Which UTXO set hash should be calculated. Options: 'hash\_serialized\_2' (the legacy algorithm), 'none'.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "gettxoutsetinfo",
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
