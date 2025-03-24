---
description: >-
  Example code for the getblock json-rpc method. Сomplete guide on how to use
  getblock json-rpc in GetBlock.io Web3 documentation.
---

# getblock - DigiByte

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
"params": ["8aef53e812659444b72dfa021ccbfddfb795e04889788c8dee802113e186acf3", 1],
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
        "pow_algo": "scrypt",
        "pow_algo_id": 1,
        "pow_hash": "0000000000006aeb302a702c61ffa2e2c339fa9f4677bc724b799bf01f742780",
        "previousblockhash": "e4d334806799cd7de69800a5aae687ee934c5e6992269209f39d5440cb54b751",
        "size": 710,
        "strippedsize": 458,
        "time": 1631261485,
        "tx": [
            "4f0934ca5affc6069dc2a112357ef7a2afcdc40307f81ebeccd86e23fddfa2db",
            "b553f7cfdb5d58eadac03aa8a7937b5aba8880c535c92850147b098cac51ec34"
        ],
        "version": 536870914,
        "versionHex": "20000002",
        "weight": 2084
    }
}
```
