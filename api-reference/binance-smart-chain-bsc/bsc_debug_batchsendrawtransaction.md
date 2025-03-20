---
description: >-
  Example code for the debug_batchSendRawTransaction  {disallowed} json-rpc
  method. Сomplete guide on how to use debug_batchSendRawTransaction 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# debug\_batchSendRawTransaction {disallowed} - Binance Smart Chain

#### Parameters

`data` - None

The signed transaction data array.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_batchSendRawTransaction",
"params": [["0xc48fb64230a82f65a08e7280bd8745e7fea87bc7c206309dee32209fe9a985f7"]],
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
