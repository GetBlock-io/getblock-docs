---
description: >-
  Example code for the state_getPairs  {disallowed} json-rpc method. Сomplete
  guide on how to use state_getPairs  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# state\_getPairs {disallowed} - Polkadot

#### Parameters

`prefix` - StorageKey

storage key

`at` - BlockHash

block hash

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "state_getPairs",
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
