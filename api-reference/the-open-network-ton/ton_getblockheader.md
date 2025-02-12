---
description: >-
  Example code for the /getBlockHeader json-rpc method. Сomplete guide on how to
  use /getBlockHeader json-rpc in GetBlock.io Web3 documentation.
---

# /getBlockHeader - The Open Network (TON)

#### Parameters

`workchain` - query

required, integer

block workchain id

`shard` - query

required, integer

block shard id

`seqno` - query

required, integer

block seqno

`root_hash` - query

optional, string

`file_hash` - query

optional, string

#### Request

```java
curl --location --request GET 'https://ton.getblock.io/mainnet//getBlockHeader?workchain=-1&shard=-9223372036854775808&seqno=30497145&root_hash=TqVSpe+5Dxx+RnBdvJ/tJeg13/nAAh9mjR/C+Jy4mmM=' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json'
```

#### Response

```java
{
    "ok": true,
    "result": {
        "@extra": "1687338546.7723587:2:0.5271371713556018",
        "@type": "blocks.header",
        "after_merge": false,
        "after_split": false,
        "before_split": false,
        "catchain_seqno": 449608,
        "end_lt": "38636875000004",
        "flags": 1,
        "gen_utime": 1687271330,
        "global_id": -239,
        "id": {
            "@type": "ton.blockIdExt",
            "file_hash": "t81TFPL17RLgqteZsYf64yY2EX18p/V5f3Z9itK1DrA=",
            "root_hash": "2a8ZfAc+YN+hWDxtnlj+lsmPxDljA1ur8XVvL52UB2s=",
            "seqno": 30497145,
            "shard": "-9223372036854775808",
            "workchain": -1
        },
        "is_key_block": false,
        "min_ref_mc_seqno": 30497141,
        "prev_blocks": [
            {
                "@type": "ton.blockIdExt",
                "file_hash": "/XCWyXIZVy+Bg3AxI5km0IP6bnsmmbI9bSQDO09m3Lw=",
                "root_hash": "i84CdB9iTlC/rsy3kBG+RF3TTWIyG358LxHgaEbCxkA=",
                "seqno": 30497144,
                "shard": "-9223372036854775808",
                "workchain": -1
            }
        ],
        "prev_key_block_seqno": 30488948,
        "start_lt": "38636875000000",
        "validator_list_hash_short": 579715162,
        "version": 0,
        "vert_seqno": 1,
        "want_merge": true,
        "want_split": false
    }
}
```
