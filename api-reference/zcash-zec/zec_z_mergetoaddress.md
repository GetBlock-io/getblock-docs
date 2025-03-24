---
description: >-
  Example code for the z_mergetoaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use z_mergetoaddress  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# z\_mergetoaddress {disallowed} - Zcash

#### Parameters

`fromaddresses` - array

A JSON array with addresses.

While it is possible to use a variety of different combinations of addresses and the above values, it is not possible to send funds from both sprout and sapling addresses simultaneously. If a special string is given, any given addresses of that type will be counted as duplicates and cause an error.

`toaddress` - string

The taddr or zaddr to send the funds to.

`fee` - numeric

Optional, default=0.00001

The fee amount to attach to this transaction.

`transparent_limit` - numeric

Optional, default=50

Limit on the maximum number of UTXOs to merge. Set to 0 to use as many as will fit in the transaction.

`shielded_limit` - numeric

Optional, default is 20 Sprout or 200 Sapling Notes

Limit on the maximum number of notes to merge. Set to 0 to merge as many as will fit in the transaction.

`memo` - string

Optional

Encoded as hex. When toaddress is a zaddr, this will be stored in the memo field of the new note.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_mergetoaddress",
"params": [null, null, null, null, null, null],
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
