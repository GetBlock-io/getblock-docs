---
description: >-
  Example code for the priv_getCode  {disallowed} json-rpc method. Сomplete
  guide on how to use priv_getCode  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# priv\_getCode {disallowed} - Ethereum Classic

#### Parameters

`data` - None

32-byte privacy Group ID.

`data` - None

20-byte contract address.

`quantity|tag` - None

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "priv_getCode",
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
