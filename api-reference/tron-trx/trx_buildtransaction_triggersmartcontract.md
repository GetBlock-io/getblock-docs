---
description: >-
  Example code for the buildTransaction(TriggerSmartContract)  {disallowed}
  json-rpc method. Сomplete guide on how to use
  buildTransaction(TriggerSmartContract)  {disallowed} json-rpc in GetBlock.io
  Web
---

# buildTransaction(TriggerSmartContract) {disallowed} - TRON

#### Parameters

`from` - data, 20 bytes

The address the transaction is sent from.

`to` - DATA, 20 bytes

The address of the smart contract

`data` - DATA

The invoked contract function and parameters

`gas` - DATA

fee limit

`value` - DATA

The data passed through call\_value.

`tokenId` - QUANTITY

Token ID

`tokenValue` - QUANTITY

The transfer amount of TRC10

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "buildTransaction",
"params": ["0xC4DB2C9DFBCB6AA344793F1DDA7BD656598A06D8", "0xf859b5c93f789f4bcffbe7cc95a71e28e5e6a5bd", "0x3be9ece7000000000000000000000000ba8e28bdb6e49fbb3f5cd82a9f5ce8363587f1f600000000000000000000000000000000000000000000000000000000000f42630000000000000000000000000000000000000000000000000000000000000001", "0x245498", "0xA", "1000035", 20],
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
