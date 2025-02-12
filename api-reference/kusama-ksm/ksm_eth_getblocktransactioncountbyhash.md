---
description: >-
  Example code for the eth_getBlockTransactionCountByHash  {disallowed} json-rpc
  method. Сomplete guide on how to use eth_getBlockTransactionCountByHash 
  {disallowed} json-rpc in GetBlock.io Web3 docume
---

# eth\_getBlockTransactionCountByHash {disallowed} - Kusama

#### Parameters

`hash` - H256

None

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockTransactionCountByHash",
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
