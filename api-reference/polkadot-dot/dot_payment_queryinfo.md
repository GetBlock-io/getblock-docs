---
description: >-
  Example code for the payment_queryInfo  {disallowed} json-rpc method. Сomplete
  guide on how to use payment_queryInfo  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# payment\_queryInfo {disallowed} - Polkadot

#### Parameters

`extrinsic` - Bytes

None

`at` - BlockHash

None

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "payment_queryInfo",
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
