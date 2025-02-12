---
description: >-
  Example code for the priv_getEeaTransactionCount  {disallowed} json-rpc
  method. Сomplete guide on how to use priv_getEeaTransactionCount  {disallowed}
  json-rpc in GetBlock.io Web3 documentation.
---

# priv\_getEeaTransactionCount {disallowed} - Ethereum Classic

#### Parameters

`data` - None

Account address.

`data` - None

Base64 encoded Orion address of the sender.

`array of data` - None

Base64 encoded Orion addresses of recipients.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "priv_getEeaTransactionCount",
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
