---
description: >-
  Example code for the createmultisig  {disallowed} json-rpc method. Сomplete
  guide on how to use createmultisig  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# createmultisig {disallowed} - Zcash

#### Parameters

`nrequired` - numeric

The number of required signatures out of the n keys or addresses.

`keys` - string

A json array of keys which are Zcash addresses or hex-encoded public keys.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "createmultisig",
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
