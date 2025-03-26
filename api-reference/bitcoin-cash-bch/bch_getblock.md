---
description: >-
  Example code for the getblock json-rpc method. Сomplete guide on how to use
  getblock json-rpc in GetBlock.io Web3 documentation.
---

# getblock - Bitcoin Cash

#### Parameters

`blockhash` - string, required

The block hash

`verbosity` - numeric, optional, default=1

0 for hex-encoded data, 1 for a json object, and 2 for json object with transaction data

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblock",
"params": [],
"id" : "getblock.io"}'
```

#### Response

```java
{
    "bits": "170cdf6f",
    "chainwork": "00000000000000000000000000000000000000001b633a711a2334c78a29bb40",
    "confirmations": 1197,
    "difficulty": 21865558044610.55,
    "hash": "000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428",
    "height": 677119,
    "mediantime": 1617176373,
    "merkleroot": "d14c9f467c4bdd5135837696150ab5f52f3f5043de324ca4e5766b195b9f8f37",
    "nTx": 2815,
    "nextblockhash": "000000000000000000006d8e1eb870bd281b30ed621acf6b8d6af2a3c7ab61f1",
    "nonce": 3669423616,
    "previousblockhash": "0000000000000000000aec32aa6edda6c888e8d6a0183d9c976064f98430c2da",
    "size": 1350854,
    "strippedsize": 882816,
    "time": 1617180599,
    "tx": [
        "2cd23815f632a3fc8a5c191c9d4fb4d758d85af4599ecd0f7f55427ad682f142",
        "780791bb2d5a8ccda4b5a707967a8e15b412814852c58c77299e85579bb65587",
        "635d50c5be6d6aea70527237dce8b0d646b88970ba34384f2ef85eba37f9e604",
        "fa8b28b42f11aa86ff1bbe6acf67b8e2e497573bef07a8e2fd5de8d79f2df80c"
    ],
    "version": 1073733632,
    "versionHex": "3fffe000",
    "weight": 3999302
}
```
