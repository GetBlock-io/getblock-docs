---
description: >-
  Example code for the addmultisigaddress  {disallowed} json-rpc method.
  Сomplete guide on how to use addmultisigaddress  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# addmultisigaddress {disallowed} - Bitcoin SV

#### Parameters

`nrequired` - numeric, required

The number of required signatures out of the n keys or addresses.

`keys` - json array, required

The bitcoin addresses or hex-encoded public keys

`label` - string, optional

A label to assign the addresses to.

`address_type` - string, optional

A label to assign the addresses to.

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "addmultisigaddress",
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
