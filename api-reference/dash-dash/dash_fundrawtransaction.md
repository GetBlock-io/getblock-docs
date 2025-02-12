---
description: >-
  Example code for the fundrawtransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use fundrawtransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# fundrawtransaction {disallowed} - Dash

#### Parameters

`Hex string` - string (hex)

The hex string of the raw transaction.

`Options` - object

Optional.

Additional options.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "fundrawtransaction",
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
