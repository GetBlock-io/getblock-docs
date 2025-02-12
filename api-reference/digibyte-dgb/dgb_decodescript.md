---
description: >-
  Example code for the decodescript json-rpc method. Сomplete guide on how to
  use decodescript json-rpc in GetBlock.io Web3 documentation.
---

# decodescript - DigiByte

#### Parameters

`hexstring` - string, required

the hex-encoded script

#### Request

```java
curl --location --request POST 'https://dgb.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "decodescript",
"params": ["76a9146f5cf1be10f3ad794eca67821fb280305b2900a188ac"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "addresses": [
            "DFHvtSCQRjfeEHX7JFhJ3JML8BihQ5DNMz"
        ],
        "asm": "OP_DUP OP_HASH160 6f5cf1be10f3ad794eca67821fb280305b2900a1 OP_EQUALVERIFY OP_CHECKSIG",
        "p2sh": "SYyCwovNwAVMBLimk8DKQEhiy3ArKsYuKJ",
        "reqSigs": 1,
        "segwit": {
            "addresses": [
                "dgb1qdaw0r0ss7wkhjnk2v7pplv5qxpdjjq9pcufk9y"
            ],
            "asm": "0 6f5cf1be10f3ad794eca67821fb280305b2900a1",
            "hex": "00146f5cf1be10f3ad794eca67821fb280305b2900a1",
            "p2sh-segwit": "SYGCCpe8NaVYHMPJzEGdh5pz1cqZNdWYf9",
            "reqSigs": 1,
            "type": "witness_v0_keyhash"
        },
        "type": "pubkeyhash"
    }
}
```
