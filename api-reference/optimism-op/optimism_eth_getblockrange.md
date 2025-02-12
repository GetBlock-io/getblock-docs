---
description: >-
  Example code for the eth_getBlockRange  {disallowed} json-rpc method. Сomplete
  guide on how to use eth_getBlockRange  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_getBlockRange {disallowed} - Optimism

#### Parameters

`QUANTITY|TAG` - string

integer of the starting block number for the range, or the string "earliest", "latest" or "pending"

`QUANTITY|TAG` - string

integer of the ending block number for the range, or the string "earliest", "latest" or "pending"

`Boolean` - Boolean

If true it returns the full transaction objects, if false only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://optimism.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockRange",
"params": ["earliest", "latest", false],
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
