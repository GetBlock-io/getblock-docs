---
description: >-
  Example code for the generatetoaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use generatetoaddress  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# generatetoaddress {disallowed} - Dash

#### Parameters

`blocks` - number (int)

The number of blocks to generate. The RPC call will not return until all blocks have been generated or the maximum number of iterations has been reached.

`address` - string (base58)

The address that will receive the newly generated Dash.

`Maxtries` - number (int)

Optional.

The maximum number of iterations that are tried to create the requested number of blocks. Default is 1000000.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "generatetoaddress",
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
