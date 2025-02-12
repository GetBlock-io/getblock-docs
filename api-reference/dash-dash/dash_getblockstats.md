---
description: >-
  Example code for the getblockstats json-rpc method. Сomplete guide on how to
  use getblockstats json-rpc in GetBlock.io Web3 documentation.
---

# getblockstats - Dash

#### Parameters

`hash_or_height` - string or numeric

The block hash or height of the target block.

`stats` - array

Optional.

Values to plot, by default all values.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockstats",
"params": ["00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3", null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "avgfee": 1354,
        "avgfeerate": 3,
        "avgtxsize": 340,
        "blockhash": "00000000000000093711aeacfe1a827cb43c6d626230cdd2e41ad6f43c1e79d3",
        "height": 1535345,
        "ins": 12,
        "maxfee": 4520,
        "maxfeerate": 20,
        "maxtxsize": 555,
        "medianfee": 522,
        "medianfeerate": 1,
        "mediantime": 1631177973,
        "mediantxsize": 260,
        "minfee": 0,
        "minfeerate": 0,
        "mintxsize": 225,
        "outs": 18,
        "subsidy": 267867322,
        "time": 1631178288,
        "total_out": 57900846913,
        "total_size": 2382,
        "totalfee": 9478,
        "txs": 8,
        "utxo_increase": 6,
        "utxo_size_inc": 450
    }
}
```
