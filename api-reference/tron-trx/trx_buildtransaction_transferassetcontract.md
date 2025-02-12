---
description: >-
  Example code for the buildTransaction(TransferAssetContract)  {disallowed}
  json-rpc method. Сomplete guide on how to use
  buildTransaction(TransferAssetContract)  {disallowed} json-rpc in GetBlock.io
  W
---

# buildTransaction(TransferAssetContract) {disallowed} - TRON

#### Parameters

`from` - data, 20 bytes

The address the transaction is sent from.

`to` - data, 20 bytes

The address the transaction is directed to.

`tokenId` - QUANTITY

token ID

`tokenValue` - QUANTITY

The transfer amount of TRC10

#### Request

```java
curl --location --request POST 'https://trx.getblock.io/mainnet/jsonrpc' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "buildTransaction",
"params": ["0xC4DB2C9DFBCB6AA344793F1DDA7BD656598A06D8", "0x95FD23D3D2221CFEF64167938DE5E62074719E54", "1000016", 20],
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
