---
description: >-
  Example code for the getFeeCalculatorForBlockhash  {disallowed} json-rpc
  method. Сomplete guide on how to use getFeeCalculatorForBlockhash 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# getFeeCalculatorForBlockhash {disallowed} - Solana

#### Parameters

`blockhash` - string

query blockhash as a Base58 encoded string

`commitment` - object

Optional

#### Request

```java
curl --location --request POST 'https://sol.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getFeeCalculatorForBlockhash",
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
