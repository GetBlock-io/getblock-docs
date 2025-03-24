---
description: >-
  Example code for the buildTransaction(TransferContract)  {disallowed} json-rpc
  method. Сomplete guide on how to use buildTransaction(TransferContract) 
  {disallowed} json-rpc in GetBlock.io Web3 docume
---

# buildTransaction(TransferContract) {disallowed} - TRON

#### Parameters

`from` - data, 20 bytes

The address the transaction is sent from.

`to` - data, 20 bytes

The address the transaction is directed to.

`value` - data

the transfer amount

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "buildTransaction",
"params": ["0xC4DB2C9DFBCB6AA344793F1DDA7BD656598A06D8", "0x95FD23D3D2221CFEF64167938DE5E62074719E54", "0x1f4"],
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
