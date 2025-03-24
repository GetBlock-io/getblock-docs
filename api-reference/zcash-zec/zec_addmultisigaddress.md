---
description: >-
  Example code for the addmultisigaddress  {disallowed} json-rpc method.
  Сomplete guide on how to use addmultisigaddress  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# addmultisigaddress {disallowed} - Zcash

#### Parameters

`nrequired` - numeric

The number of required signatures out of the n keys or addresses.

`keysobject` - string

A json array of Zcash addresses or hex-encoded public keys.

`account` - string

DEPRECATED. If provided, MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "addmultisigaddress",
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
