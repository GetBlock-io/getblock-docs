---
description: >-
  Example code for the getblock json-rpc method. Сomplete guide on how to use
  getblock json-rpc in GetBlock.io Web3 documentation.
---

# getblock - Bitcoin Gold

#### Parameters

`blockhash` - string, required

The block hash

`verbosity` - numeric, optional, default=1

0 for hex-encoded data, 1 for a json object, and 2 for json object with transaction data

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblock",
"params": ["000000009ec5dd6b53858593718cacdaaec989aaf9028c68013947224712682e", 1],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bits": "1d0364c9",
        "chainwork": "000000000000000000000000000000000000000000a21782bdfc4cff2792f60e",
        "confirmations": 117,
        "difficulty": 154488.6800084529,
        "hash": "000000009ec5dd6b53858593718cacdaaec989aaf9028c68013947224712682e",
        "height": 702518,
        "mediantime": 1630942082,
        "merkleroot": "5c970c064d317658a8777ee56f192fe17d5b3afbe242d820b205c2291210f269",
        "nTx": 1,
        "nextblockhash": "00000000ec519c94560bfa8da64b5d69f479c252c7677ed9a3700e7cde43ee4a",
        "nonce": "00009c3a000000000000000000000000000000000000000000000000f6460004",
        "nonceUint32": 4131782660,
        "previousblockhash": "00000000f25590d5d83d75eadab4f4b42439bfe924a2c3752bcdf5a8f734f83b",
        "size": 460,
        "solution": "0c8906341016d6e8e8d1fc1f85493065a530ea0f3bbbbe2aa52326f6d4c06920bcbc35b77657b06ef7073f4e28512ff593480d476bcd5bb44c1e510f0af2a525a002f85515683d9d2dcfee0f669e1b133d0b9610bb3ac1043c45262fc811c75b8170e1c5",
        "strippedsize": 424,
        "time": 1630943363,
        "tx": [
            "5c970c064d317658a8777ee56f192fe17d5b3afbe242d820b205c2291210f269"
        ],
        "version": 536870912,
        "versionHex": "20000000",
        "weight": 1732
    }
}
```
