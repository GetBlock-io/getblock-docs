---
description: >-
  Example code for the prioritisetransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use prioritisetransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# prioritisetransaction {disallowed} - Zcash

#### Parameters

`txid` - string

The transaction id.

`priority delta` - numeric

The priority to add or subtract.

The transaction selection algorithm considers the tx as it would have a higher priority.

(priority of a transaction is calculated: coinage \* value\_in\_satoshis / txsize)

`fee delta` - numeric

The fee value (in satoshis) to add (or subtract, if negative).

The fee is not actually paid, only the algorithm for selecting transactions into a block considers the transaction as it would have paid a higher (or lower) fee.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "prioritisetransaction",
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
