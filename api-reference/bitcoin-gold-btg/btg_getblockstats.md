---
description: >-
  Example code for the getblockstats json-rpc method. Сomplete guide on how to
  use getblockstats json-rpc in GetBlock.io Web3 documentation.
---

# getblockstats - Bitcoin Gold

#### Parameters

`hash_or_height` - string or numeric, required

The block hash or height of the target block

`stats` - json array, optional, default=all values

Values to plot (see result below)

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblockstats",
"params": [702518, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "avgfee": 0,
        "avgfeerate": 0,
        "avgtxsize": 0,
        "blockhash": "000000009ec5dd6b53858593718cacdaaec989aaf9028c68013947224712682e",
        "feerate_percentiles": [
            0,
            0,
            0,
            0,
            0
        ],
        "height": 702518,
        "ins": 0,
        "maxfee": 0,
        "maxfeerate": 0,
        "maxtxsize": 0,
        "medianfee": 0,
        "mediantime": 1630942082,
        "mediantxsize": 0,
        "minfee": 0,
        "minfeerate": 0,
        "mintxsize": 0,
        "outs": 2,
        "subsidy": 625000000,
        "swtotal_size": 0,
        "swtotal_weight": 0,
        "swtxs": 0,
        "time": 1630943363,
        "total_out": 0,
        "total_size": 0,
        "total_weight": 0,
        "totalfee": 0,
        "txs": 1,
        "utxo_increase": 2,
        "utxo_size_inc": 163
    }
}
```
