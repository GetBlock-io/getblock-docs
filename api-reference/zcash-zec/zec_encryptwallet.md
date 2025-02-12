---
description: >-
  Example code for the encryptwallet  {disallowed} json-rpc method. Сomplete
  guide on how to use encryptwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# encryptwallet {disallowed} - Zcash

#### Parameters

`passphrase` - string

The pass phrase to encrypt the wallet with. It must be at least 1 character, but should be long.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "encryptwallet",
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
