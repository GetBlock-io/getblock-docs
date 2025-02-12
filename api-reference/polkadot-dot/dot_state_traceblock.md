---
description: >-
  Example code for the state_traceBlock  {disallowed} json-rpc method. Сomplete
  guide on how to use state_traceBlock  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# state\_traceBlock {disallowed} - Polkadot

#### Parameters

`block` - Hash

block hash

`targets` - Option Text

target

`StorageKey` - Option Text

storage key

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "state_traceBlock",
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
