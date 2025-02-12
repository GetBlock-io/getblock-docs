---
description: >-
  Example code for the createmultisig  {disallowed} json-rpc method. Сomplete
  guide on how to use createmultisig  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# createmultisig {disallowed} - Dash

#### Parameters

`required` - number (int)

The minimum (m) number of signatures required to spend this m-of-n multisig script

`keys` - array

An array of strings with each string being a public key.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "createmultisig",
"params": [null, [{"name": "key", "type": "string", "description": ["A public key against which signatures will be checked. There must be at least as many keys as specified by the Required parameter, and there may be more keys."], "value": null}]],
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
