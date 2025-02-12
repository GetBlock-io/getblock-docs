---
description: >-
  Example code for the getblockheader json-rpc method. Сomplete guide on how to
  use getblockheader json-rpc in GetBlock.io Web3 documentation.
---

# getblockheader - Bitcoin Cash

#### Parameters

`blockhash` - string, required

The block hash

`verbose` - boolean, optional, default=true

true for a json object, false for the hex-encoded data

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblockheader",
"params": ["000000000000000000046b9302e08c16ea186950f42a5498320ddd1bd7ab3428", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
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
        "time": 1617180599,
        "version": 1073733632,
        "versionHex": "3fffe000"
    }
}
```
