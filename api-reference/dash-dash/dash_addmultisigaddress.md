---
description: >-
  Example code for the addmultisigaddress  {disallowed} json-rpc method.
  Сomplete guide on how to use addmultisigaddress  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# addmultisigaddress {disallowed} - Dash

#### Parameters

`Required` - number (int)

The minimum (m) number of signatures required to spend this m-of-n multisig script.

`Keys Or Addresses` - array

An array of strings with each string being a public key or address

`Label` - string

Optional.

A label to assign the addresses to.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "addmultisigaddress",
"params": [null, [{"name": "Key Or Address", "type": "string", "description": ["A public key against which signatures will be checked. Alternatively, this may be a P2PKH address belonging to the wallet---the corresponding public key will be substituted. There must be at least as many keys as specified by the Required parameter, and there may be more keys."], "value": null}], null],
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
