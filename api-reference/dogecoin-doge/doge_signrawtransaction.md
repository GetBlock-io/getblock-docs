---
description: >-
  Example code for the signrawtransaction json-rpc method. Сomplete guide on how
  to use signrawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# signrawtransaction - Dogecoin

#### Parameters

`hexstring` - string

A hex string of the transaction to sign.

`previous_transactions` - list of dict

Optional

list of dictionaries of the form: { “txid”: txid, “vout”: n, “scriptPubKey”: hex, “redeemScript”: hex } representing previous transaction outputs that this transaction depends on but may not yet be in the block chain.

`private_keys` - list of string

Optional

A list of base58-encoded private keys that, if given, will be the only keys used to sign the transaction.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "signrawtransaction",
"params": [null, [{"txid": "10c03013c4feadd7d2e0d948240771dbbc220152d12c3ab7628a40a7f827f658", "vout": 10, "scriptPubKey": "hexencodedscriptpubkey", "redeemscript": "hexencodedredeemscript"}, {"txid": "10c01013c4feadd7d2e0d948240771dbbc220152d12c3ab7628a40a7f827f658", "vout": 1, "scriptPubKey": "hexencodedscriptpubkey", "redeemscript": "hexencodedredeemscript"}], null],
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
