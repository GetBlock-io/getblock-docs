---
description: >-
  Example code for the gettxout json-rpc method. Сomplete guide on how to use
  gettxout json-rpc in GetBlock.io Web3 documentation.
---

# gettxout - Dogecoin

#### Parameters

`txid` - string

Transactiond id for which the info should be returned.

`index` - string

The output index.

`mempool` - boolean

Optional, default=true

Add memory pool transactions.

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "gettxout",
"params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7", 0, true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bestblock": "b1a5c36e7d007c5592061187d1e1eb2fdc9bac0988cb8cf5b81be9c3751e4b72",
        "coinbase": true,
        "confirmations": 1389,
        "scriptPubKey": {
            "addresses": [
                "D5gVXk2rNW7r1iBfMzfE77nhC5RdhiMi4u"
            ],
            "asm": "OP_DUP OP_HASH160 05f0163a43999424760262011d2d426eedd833da OP_EQUALVERIFY OP_CHECKSIG",
            "hex": "76a91405f0163a43999424760262011d2d426eedd833da88ac",
            "reqSigs": 1,
            "type": "pubkeyhash"
        },
        "value": 10000.0,
        "version": 1
    }
}
```
