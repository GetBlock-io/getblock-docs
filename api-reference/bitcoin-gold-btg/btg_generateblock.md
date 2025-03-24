---
description: >-
  Example code for the generateblock  {disallowed} json-rpc method. Сomplete
  guide on how to use generateblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# generateblock {disallowed} - Bitcoin Gold

#### Parameters

`output` - string, required

The address or descriptor to send the newly generated btc to.

`transactions` - json array, required

An array of hex strings which are either txids or raw transactions.

Txids must reference transactions currently in the mempool.

All transactions must be valid and in valid order, otherwise the block will be rejected.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "generateblock",
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
