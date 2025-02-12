---
description: >-
  Example code for the decoderawtransaction json-rpc method. Сomplete guide on
  how to use decoderawtransaction json-rpc in GetBlock.io Web3 documentation.
---

# decoderawtransaction - Zcash

#### Parameters

`hexstring` - string

The transaction hex string

#### Request

```java
curl --location --request POST 'https://zec.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "decoderawtransaction",
"params": ["0400008085202f89010000000000000000000000000000000000000000000000000000000000000000ffffffff2003bb1e151b5c4c55584f525c000000000f4bcd54a8ae0234000000000000000000000000045246ea0e000000001976a91417b04a8ede7164eccb961f46289305ec04014b6388ac38c94d010000000017a914c1d33ded7edf633ca2592f2258d4c8c9ae28091587286bee000000000017a914d45cb1adffb5215a42720532a076f02c7c778c908740787d010000000017a914931fec54c1fea86e574462cc32013f5400b891298700000000e31e15000000000000000000000000"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "expiryheight": 1384163,
        "hex": "0400008085202f89010000000000000000000000000000000000000000000000000000000000000000ffffffff2003bb1e151b5c4c55584f525c000000000f4bcd54a8ae0234000000000000000000000000045246ea0e000000001976a91417b04a8ede7164eccb961f46289305ec04014b6388ac38c94d010000000017a914c1d33ded7edf633ca2592f2258d4c8c9ae28091587286bee000000000017a914d45cb1adffb5215a42720532a076f02c7c778c908740787d010000000017a914931fec54c1fea86e574462cc32013f5400b891298700000000e31e15000000000000000000000000",
        "locktime": 0,
        "overwintered": true,
        "size": 232,
        "txid": "5db76e43724d980e01b5b98c9a83ba2d7fa565b3aa22bb3c5728ea56d0ed7cee",
        "vShieldedOutput": [],
        "vShieldedSpend": [],
        "valueBalance": 0.0,
        "valueBalanceZat": 0,
        "version": 4,
        "versiongroupid": "892f2085",
        "vin": [
            {
                "coinbase": "03bb1e151b5c4c55584f525c000000000f4bcd54a8ae02340000000000000000",
                "sequence": 0
            }
        ],
        "vjoinsplit": [],
        "vout": [
            {
                "n": 0,
                "scriptPubKey": {
                    "addresses": [
                        "t1L2rjgGrvEqfrA5zqUca4GGxAeQg47CTpG"
                    ],
                    "asm": "OP_DUP OP_HASH160 17b04a8ede7164eccb961f46289305ec04014b63 OP_EQUALVERIFY OP_CHECKSIG",
                    "hex": "76a91417b04a8ede7164eccb961f46289305ec04014b6388ac",
                    "reqSigs": 1,
                    "type": "pubkeyhash"
                },
                "value": 2.5023445,
                "valueSat": 250234450,
                "valueZat": 250234450
            },
            {
                "n": 1,
                "scriptPubKey": {
                    "addresses": [
                        "t3cEUQFG3KYnFG6qYhPxSNgGi3HDjUPwC3J"
                    ],
                    "asm": "OP_HASH160 c1d33ded7edf633ca2592f2258d4c8c9ae280915 OP_EQUAL",
                    "hex": "a914c1d33ded7edf633ca2592f2258d4c8c9ae28091587",
                    "reqSigs": 1,
                    "type": "scripthash"
                },
                "value": 0.21875,
                "valueSat": 21875000,
                "valueZat": 21875000
            },
            {
                "n": 2,
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
                "valueSat": 15625000,
                "valueZat": 15625000
            },
            {
                "n": 3,
                "scriptPubKey": {
                    "addresses": [
                        "t3XyYW8yBFRuMnfvm5KLGFbEVz25kckZXym"
                    ],
                    "asm": "OP_HASH160 931fec54c1fea86e574462cc32013f5400b89129 OP_EQUAL",
                    "hex": "a914931fec54c1fea86e574462cc32013f5400b8912987",
                    "reqSigs": 1,
                    "type": "scripthash"
                },
                "value": 0.25,
                "valueSat": 25000000,
                "valueZat": 25000000
            }
        ]
    }
}
```
