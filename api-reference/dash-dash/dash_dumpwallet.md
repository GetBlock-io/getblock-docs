---
description: >-
  Example code for the dumpwallet  {disallowed} json-rpc method. Сomplete guide
  on how to use dumpwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# dumpwallet {disallowed} - Dash

#### Parameters

`Filename` - string

The filename with path (either absolute or relative to Dash Core) into which the wallet dump will be placed. An existing file with that name will be overwritten.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "dumpwallet",
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
