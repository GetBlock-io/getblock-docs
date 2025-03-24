---
description: >-
  Example code for the getblockheader json-rpc method. Сomplete guide on how to
  use getblockheader json-rpc in GetBlock.io Web3 documentation.
---

# getblockheader - DigiByte

#### Parameters

`blockhash` - string, required

The block hash

`verbose` - boolean, optional, default=true

true for a json object, false for the hex-encoded data

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockheader",
"params": ["8aef53e812659444b72dfa021ccbfddfb795e04889788c8dee802113e186acf3", true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "bits": "1b008d6d",
        "chainwork": "0000000000000000000000000000000000000000000c084ec5a1d70eff223bcc",
        "confirmations": 18540,
        "difficulty": 118627.3100400497,
        "hash": "8aef53e812659444b72dfa021ccbfddfb795e04889788c8dee802113e186acf3",
        "height": 13627174,
        "mediantime": 1631261365,
        "merkleroot": "cd3f02b4edcdac21fe0d2eba969e2fd5ab5a0de645d3599e9ea83467c005fa74",
        "nTx": 2,
        "nextblockhash": "19ff9b2a0471fcc2d8ad1e6788e0ef5aa5b055d0855d6a2e12accf0de8cc8d20",
        "nonce": 1413312706,
        "previousblockhash": "e4d334806799cd7de69800a5aae687ee934c5e6992269209f39d5440cb54b751",
        "time": 1631261485,
        "version": 536870914,
        "versionHex": "20000002"
    }
}
```
