---
description: >-
  Example code for the contracts_getStorage  {disallowed} json-rpc method.
  Сomplete guide on how to use contracts_getStorage  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# contracts\_getStorage {disallowed} - Polkadot

#### Parameters

`address` - AccountId

None

`key` - H256

None

`at` - BlockHash

None

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "contracts_getStorage",
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
