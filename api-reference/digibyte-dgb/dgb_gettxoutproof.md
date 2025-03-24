---
description: >-
  Example code for the gettxoutproof json-rpc method. Сomplete guide on how to
  use gettxoutproof json-rpc in GetBlock.io Web3 documentation.
---

# gettxoutproof - DigiByte

#### Parameters

`txids` - json array, required

A json array of txids to filter

`blockhash` - string, optional

If specified, looks for txid in the block with this hash

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "gettxoutproof",
"params": [["4f0934ca5affc6069dc2a112357ef7a2afcdc40307f81ebeccd86e23fddfa2db", "b553f7cfdb5d58eadac03aa8a7937b5aba8880c535c92850147b098cac51ec34"], null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": "0200002051b754cb40549df309922692695e4c93ee87e6aaa50098e67dcd99678034d3e474fa05c06734a89e9e59d345e60d5aabd52f9e96ba2e0dfe21accdedb4023fcd2d133b616d8d001bc2703d540200000002dba2dffd236ed8ccbe1ef80703c4cdafa2f77e3512a1c29d06c6ff5aca34094f34ec51ac8c097b145028c935c58088ba5a7b93a7a83ac0daea585ddbcff753b50107"
}
```
