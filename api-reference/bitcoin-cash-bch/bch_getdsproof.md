---
description: >-
  Example code for the getdsproof  {disallowed} json-rpc method. Сomplete guide
  on how to use getdsproof  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getdsproof {disallowed} - Bitcoin Cash

#### Parameters

`dspid_or_txid_or_outpoint` - string, required

The dspid, txid, or output point associated with the double-spend proof you wish to retrieve. Outpoints should be specified as a json object containing keys "txid" (string) and "vout" (numeric).

`verbosity` - numeric, optional, default=2

Values 0-3 return progressively more information for each increase in verbosity. This option may also be specified as a boolean where false is the same as verbosity=0 and true is verbosity=2.

`recursive` - boolean, optional, default=true

If doing a lookup by txid, then search for a double-spend proof for all in-mempool ancestors of txid as well. This option is ignored if not searching by txid.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getdsproof",
"params": ["fb5ae5344cb6995e529201fe24247ac38452f4e5ab5669b649e935853a7a180a", null, null],
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
