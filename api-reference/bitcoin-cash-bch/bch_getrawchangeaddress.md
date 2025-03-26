---
description: >-
  Example code for the getrawchangeaddress  {disallowed} json-rpc method.
  Сomplete guide on how to use getrawchangeaddress  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getrawchangeaddress {disallowed} - Bitcoin Cash

#### Parameters

`address_type` - string, optional, default=set by -changetype

The address type to use. Options are “legacy”, “p2sh-segwit”, and “bech32”.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getrawchangeaddress",
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
