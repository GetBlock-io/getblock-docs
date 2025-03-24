---
description: >-
  Example code for the get_block_header_by_height json-rpc method. Сomplete
  guide on how to use get_block_header_by_height json-rpc in GetBlock.io Web3
  documentation.
---

# get\_block\_header\_by\_height - Monero

#### Parameters

`height` - unsigned int

The block's height.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "get_block_header_by_height",
"params": {"height": 912345},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "block_header": {
            "block_size": 210,
            "block_weight": 210,
            "cumulative_difficulty": 754734824984346,
            "cumulative_difficulty_top64": 0,
            "depth": 1482236,
            "difficulty": 815625611,
            "difficulty_top64": 0,
            "hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
            "height": 912345,
            "long_term_weight": 210,
            "major_version": 1,
            "miner_tx_hash": "c7da3965f25c19b8eb7dd8db48dcd4e7c885e2491db77e289f0609bf8e08ec30",
            "minor_version": 2,
            "nonce": 1646,
            "num_txes": 0,
            "orphan_status": false,
            "pow_hash": "",
            "prev_hash": "b61c58b2e0be53fad5ef9d9731a55e8a81d972b8d90ed07c04fd37ca6403ff78",
            "reward": 7388968946286,
            "timestamp": 1452793716,
            "wide_cumulative_difficulty": "0x2ae6d65248f1a",
            "wide_difficulty": "0x309d758b"
        },
        "credits": 0,
        "status": "OK",
        "top_hash": "",
        "untrusted": false
    }
}
```
