---
description: >-
  Example code for the sendrawtransaction json-rpc method. Сomplete guide on how
  to use sendrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# sendrawtransaction - Zcash

#### Parameters

`hexstring` - string

The hex string of the raw transaction

`allowhighfees` - boolean

Optional, default=false

Whether allow high fees or not.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sendrawtransaction",
"params": ["0400008085202f89010000000000000000000000000000000000000000000000000000000000000000ffffffff2003bb1e151b5c4c55584f525c000000000f4bcd54a8ae0234000000000000000000000000045246ea0e000000001976a91417b04a8ede7164eccb961f46289305ec04014b6388ac38c94d010000000017a914c1d33ded7edf633ca2592f2258d4c8c9ae28091587286bee000000000017a914d45cb1adffb5215a42720532a076f02c7c778c908740787d010000000017a914931fec54c1fea86e574462cc32013f5400b891298700000000e31e15000000000000000000000000", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "0400008085202f89010000000000000000000000000000000000000000000000000000000000000000ffffffff2003bb1e151b5c4c55584f525c000000000f4bcd54a8ae0234000000000000000000000000045246ea0e000000001976a91417b04a8ede7164eccb961f46289305ec04014b6388ac38c94d010000000017a914c1d33ded7edf633ca2592f2258d4c8c9ae28091587286bee000000000017a914d45cb1adffb5215a42720532a076f02c7c778c908740787d010000000017a914931fec54c1fea86e574462cc32013f5400b891298700000000e31e15000000000000000000000000"
}
```
