---
description: >-
  Example code for the gettransaction json-rpc method. Сomplete guide on how to
  use gettransaction json-rpc in GetBlock.io Web3 documentation.
---

# gettransaction - Dogecoin

#### Parameters

`txid` - string

Transactiond id for which the info should be returned

#### Request

```java
curl --location --request POST 'https://doge.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "gettransaction",
"params": ["8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "blockhash": "24f64485ea05250ce75a73b3549afad05e078a7d8abd54e695a957dadee264fa",
        "blocktime": 1632136777,
        "confirmations": 1389,
        "hash": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7",
        "hex": "01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff060314953b0101ffffffff010010a5d4e80000001976a91405f0163a43999424760262011d2d426eedd833da88ac00000000",
        "locktime": 0,
        "size": 91,
        "time": 1632136777,
        "txid": "8e992e1512887da5e02fd52a1bd19d527ee54436934c215f0c23cf4ec04771e7",
        "version": 1,
        "vin": [
            {
                "coinbase": "0314953b0101",
                "sequence": 4294967295
            }
        ],
        "vout": [
            {
                "n": 0,
                "scriptPubKey": {
                    "addresses": [
                        "D5gVXk2rNW7r1iBfMzfE77nhC5RdhiMi4u"
                    ],
                    "asm": "OP_DUP OP_HASH160 05f0163a43999424760262011d2d426eedd833da OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a91405f0163a43999424760262011d2d426eedd833da88ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash"
                },
                "value": 10000.0
            }
        ],
        "vsize": 91
    }
}
```
