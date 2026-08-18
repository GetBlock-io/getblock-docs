---
description: >-
  Example code for the priv_call  {disallowed} json-rpc method. Сomplete guide
  on how to use priv_call  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# priv\_call {disallowed} - Ethereum Classic

#### Parameters

`data` - None

32-byte privacy Group ID.

`object` - None

Transaction call object.

`quantity|tag` - None

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "priv_call",
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
