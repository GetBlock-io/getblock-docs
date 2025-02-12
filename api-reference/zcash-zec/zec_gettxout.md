---
description: >-
  Example code for the gettxout json-rpc method. Сomplete guide on how to use
  gettxout json-rpc in GetBlock.io Web3 documentation.
---

# gettxout - Zcash

#### Parameters

`txid` - string

The transaction id.

`n` - numeric

Vout value

`includemempool` - boolean

Optional

Whether to include the mempool.

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "gettxout",
"params": ["5db76e43724d980e01b5b98c9a83ba2d7fa565b3aa22bb3c5728ea56d0ed7cee", 2, false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bestblock": "00000000019af1612080b8d6b3ce87c5b7569a114e6a77385260cf45a6419fe1",
        "coinbase": true,
        "confirmations": 236,
        "scriptPubKey": {
            "addresses": [
                "t3dvVE3SQEi7kqNzwrfNePxZ1d4hUyztBA1"
            ],
            "asm": "OP_HASH160 d45cb1adffb5215a42720532a076f02c7c778c90 OP_EQUAL",
            "hex": "a914d45cb1adffb5215a42720532a076f02c7c778c9087",
            "reqSigs": 1,
            "type": "scripthash"
        },
        "value": 0.15625,
        "version": 4
    }
}
```
