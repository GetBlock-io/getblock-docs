---
description: >-
  Example code for the decoderawtransaction json-rpc method. Сomplete guide on
  how to use decoderawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# decoderawtransaction - Dash

#### Parameters

`Serialized Transaction` - string (hex)

The transaction to decode in serialized transaction format.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "decoderawtransaction",
"params": ["03000500010000000000000000000000000000000000000000000000000000000000000000ffffffff2703716d170423ce39610800004440830900000fe4b883e5bda9e7a59ee4bb99e9b1bc04f09f909f40440fa802203d5807000000001976a9147c086eada12bdb10a265c16c08a7ae87366bd48188aca03c9f08000000001976a91406c7111117f7b797528485b64772d3ffcff919ec88ac209af41f460200716d1700efc371b5251f5bae393e5962fe092f8b2003732a56eda3e1a2babe8413d17ce7ce2396a41c1f833c0cd00a0d8e900dfc4962805706e70a35074dcd30fafbd4c6"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "cbTx": {
            "height": 1535345,
            "merkleRootMNList": "e77cd11384bebaa2e1a3ed562a7303208b2f09fe62593e39ae5b1f25b571c3ef",
            "merkleRootQuorums": "c6d4fbfa30cd4d07350ae70657806249fc0d908e0d0ad00c3c831f1ca49623ce",
            "version": 2
        },
        "extraPayload": "0200716d1700efc371b5251f5bae393e5962fe092f8b2003732a56eda3e1a2babe8413d17ce7ce2396a41c1f833c0cd00a0d8e900dfc4962805706e70a35074dcd30fafbd4c6",
        "extraPayloadSize": 70,
        "locktime": 536123936,
        "size": 229,
        "txid": "83dc6c8e03026c0317885f62a7072dfde10014967f59477a0f7b5fc52f44a784",
        "type": 5,
        "version": 3,
        "vin": [
            {
                "coinbase": "03716d170423ce39610800004440830900000fe4b883e5bda9e7a59ee4bb99e9b1bc04f09f909f",
                "sequence": 2819572800
            }
        ],
        "vout": [
            {
                "n": 0,
                "scriptPubKey": {
                    "addresses": [
                        "XmzfivrzYQ7B7oBMZKwPRdhjB1iNvX71XZ"
                    ],
                    "asm": "OP_DUP OP_HASH160 7c086eada12bdb10a265c16c08a7ae87366bd481 OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a9147c086eada12bdb10a265c16c08a7ae87366bd48188ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash"
                },
                "value": 1.23223328,
                "valueSat": 123223328
            },
            {
                "n": 1,
                "scriptPubKey": {
                    "addresses": [
                        "XbJgMoHvwhr6RYdGVXW28qniJ1US2Y31Ez"
                    ],
                    "asm": "OP_DUP OP_HASH160 06c7111117f7b797528485b64772d3ffcff919ec OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a91406c7111117f7b797528485b64772d3ffcff919ec88ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash"
                },
                "value": 1.44653472,
                "valueSat": 144653472
            }
        ]
    }
}
```
