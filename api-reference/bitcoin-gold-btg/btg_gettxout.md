---
description: >-
  Example code for the gettxout json-rpc method. Сomplete guide on how to use
  gettxout json-rpc in GetBlock.io Web3 documentation.
---

# gettxout - Bitcoin Gold

#### Parameters

`txid` - string, required

The transaction id

`n` - numeric, required

vout number

`include_mempool` - boolean, optional, default=true

Whether to include the mempool. Note that an unspent output that is spent in the mempool won't appear.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "gettxout",
"params": ["451f2875c30e80c3c0bbdef1089425d9547ae85eb1300b43a343f1075b98312a", 1, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": null
}
```
