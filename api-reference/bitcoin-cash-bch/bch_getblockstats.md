---
description: >-
  Example code for the getblockstats json-rpc method. Сomplete guide on how to
  use getblockstats json-rpc in GetBlock.io Web3 documentation.
---

# getblockstats - Bitcoin Cash

#### Parameters

`hash_or_height` - string or numeric, required

The block hash or height of the target block

`stats` - json array, optional, default=all values

Values to plot (see result below)

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblockstats",
"params": [1000, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "avgfee": 0.0,
        "avgfeerate": 0.0,
        "avgtxsize": 0,
        "blockhash": "00000000c937983704a73af28acdec37b049d214adbda81d7e2a3dd146f6ed09",
        "feerate_percentiles": [
            0.0,
            0.0,
            0.0,
            0.0,
            0.0
        ],
        "height": 1000,
        "ins": 0,
        "maxfee": 0.0,
        "maxfeerate": 0.0,
        "maxtxsize": 0,
        "medianfee": 0.0,
        "mediantime": 1232344831,
        "mediantxsize": 0,
        "minfee": 0.0,
        "minfeerate": 0.0,
        "mintxsize": 0,
        "outs": 1,
        "subsidy": 50.0,
        "time": 1232346882,
        "total_out": 0.0,
        "total_size": 0,
        "totalfee": 0.0,
        "txs": 1,
        "utxo_increase": 1,
        "utxo_size_inc": 117
    }
}
```
