---
description: >-
  Example code for the get_block_headers_range json-rpc method. Сomplete guide
  on how to use get_block_headers_range json-rpc in GetBlock.io Web3
  documentation.
---

# get\_block\_headers\_range - Monero

#### Parameters

`start_height` - unsigned int

The starting block's height.

`None` - unsigned int

The ending block's height.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "get_block_headers_range",
"params": {"start_height": 1545999, "end_height": 1546000},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "credits": 0,
        "headers": [
            {
                "block_size": 80,
                "block_weight": 80,
                "cumulative_difficulty": 1,
                "cumulative_difficulty_top64": 0,
                "depth": 2394584,
                "difficulty": 1,
                "difficulty_top64": 0,
                "hash": "418015bb9ae982a1975da7d79277c2705727a56894ba0fb246adaabb1f4632e3",
                "height": 0,
                "long_term_weight": 80,
                "major_version": 1,
                "miner_tx_hash": "c88ce9783b4f11190d7b9c17a69c1c52200f9faaee8e98dd07e6811175177139",
                "minor_version": 0,
                "nonce": 10000,
                "num_txes": 0,
                "orphan_status": false,
                "pow_hash": "",
                "prev_hash": "0000000000000000000000000000000000000000000000000000000000000000",
                "reward": 17592186044415,
                "timestamp": 0,
                "wide_cumulative_difficulty": "0x1",
                "wide_difficulty": "0x1"
            }
        ],
        "status": "OK",
        "top_hash": "",
        "untrusted": false
    }
}
```
