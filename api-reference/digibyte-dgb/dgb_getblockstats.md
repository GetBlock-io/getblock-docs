---
description: >-
  Example code for the getblockstats json-rpc method. Сomplete guide on how to
  use getblockstats json-rpc in GetBlock.io Web3 documentation.
---

# getblockstats - DigiByte

#### Parameters

`hash_or_height` - string or numeric, required

The block hash or height of the target block

`stats` - json array, optional, default=all values

Values to plot

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockstats",
"params": ["8aef53e812659444b72dfa021ccbfddfb795e04889788c8dee802113e186acf3", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "avgfee": 7777,
        "avgfeerate": 36,
        "avgtxsize": 373,
        "blockhash": "8aef53e812659444b72dfa021ccbfddfb795e04889788c8dee802113e186acf3",
        "feerate_percentiles": [
            36,
            36,
            36,
            36,
            36
        ],
        "height": 13627174,
        "ins": 2,
        "maxfee": 7777,
        "maxfeerate": 36,
        "maxtxsize": 373,
        "medianfee": 7777,
        "mediantime": 1631261365,
        "mediantxsize": 373,
        "minfee": 7777,
        "minfeerate": 36,
        "mintxsize": 373,
        "outs": 4,
        "subsidy": 49718262027,
        "swtotal_size": 373,
        "swtotal_weight": 844,
        "swtxs": 1,
        "time": 1631261485,
        "total_out": 148836406423,
        "total_size": 373,
        "total_weight": 844,
        "totalfee": 7777,
        "txs": 2,
        "utxo_increase": 2,
        "utxo_size_inc": 166
    }
}
```
