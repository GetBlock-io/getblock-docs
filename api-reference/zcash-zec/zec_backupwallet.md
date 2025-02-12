---
description: >-
  Example code for the backupwallet  {disallowed} json-rpc method. Сomplete
  guide on how to use backupwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# backupwallet {disallowed} - Zcash

#### Parameters

`destination` - string

The destination filename, saved in the directory set by -exportdir option.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "backupwallet",
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
