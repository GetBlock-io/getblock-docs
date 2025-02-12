---
description: >-
  Example code for the avax.getAtomicTx  {disallowed} json-rpc method. Сomplete
  guide on how to use avax.getAtomicTx  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# avax.getAtomicTx {disallowed} - Avalanche

#### Parameters

`txID` - string

txID is the transacion ID. It should be in cb58 format.

`encoding` - string

Optional encoding parameter to specify the format for the returned transaction. Can be either cb58 or hex. Defaults to cb58.

#### Request

```java
curl --location --request POST 'https://avax.getblock.io/mainnet/ext/bc/C/rpc' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "avax.getAtomicTx",
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
