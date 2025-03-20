---
description: >-
  Example code for the avax.getAtomicTxStatus  {disallowed} json-rpc method.
  Сomplete guide on how to use avax.getAtomicTxStatus  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# avax.getAtomicTxStatus {disallowed} - Avalanche

#### Parameters

`txID` - string

id of the transaction

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "avax.getAtomicTxStatus",
  "params": [null],
  "id": "getblock.io"
}'
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
