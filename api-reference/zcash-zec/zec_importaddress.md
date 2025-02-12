---
description: >-
  Example code for the importaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use importaddress  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# importaddress {disallowed} - Zcash

#### Parameters

`script` - string

The hex-encoded script (or address)

`label` - string

Optional, default=""

An optional label.

`rescan` - boolean

Optional, default=false

Rescan the wallet for transactions.

`p2sh` - boolean

Optional, default=false

Add the P2SH version of the script as well

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "importaddress",
"params": [null, null, null, null],
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
