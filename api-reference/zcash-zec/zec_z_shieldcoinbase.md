---
description: >-
  Example code for the z_shieldcoinbase  {disallowed} json-rpc method. Сomplete
  guide on how to use z_shieldcoinbase  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# z\_shieldcoinbase {disallowed} - Zcash

#### Parameters

`fromaddresses` - string

The address is a taddr or "\*" for all taddrs belonging to the wallet.

`toaddresses` - string

The address is a zaddr.

`fee` - numeric

Optional, default=0.00001

The fee amount to attach to this transaction.

`limit` - numeric

Optional, default=50

Limit on the maximum number of utxos to shield. Set to 0 to use as many as will fit in the transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_shieldcoinbase",
"params": [null, null, null, null],
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
