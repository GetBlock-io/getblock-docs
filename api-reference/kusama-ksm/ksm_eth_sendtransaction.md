---
description: >-
  Example code for the eth_sendTransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_sendTransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_sendTransaction {disallowed} - Kusama

#### Parameters

`tx` - EthTransactionRequest

None

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_sendTransaction",
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
