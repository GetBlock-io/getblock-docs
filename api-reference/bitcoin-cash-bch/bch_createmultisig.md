---
description: >-
  Example code for the createmultisig  {disallowed} json-rpc method. Сomplete
  guide on how to use createmultisig  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# createmultisig {disallowed} - Bitcoin Cash

#### Parameters

`nrequired` - numeric, required

The number of required signatures out of the n keys.

`keys` - json array, required

The hex-encoded public keys.

`address_type` - string, optional, default=legacy

The address type to use. Options are "legacy", "p2sh-segwit", and "bech32".

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "createmultisig",
"params": [null, null, null],
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
