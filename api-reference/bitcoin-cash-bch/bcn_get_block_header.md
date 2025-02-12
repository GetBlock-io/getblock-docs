---
description: >-
  Example code for the get_block_header json-rpc method. Сomplete guide on how
  to use get_block_header json-rpc in GetBlock.io Web3 documentation.
---

# get\_block\_header - Bitcoin Cash

#### Parameters

`hash` - string

Optional.

Hash of a block to get.

`height_or_depth` - int32

Optional.

Height or depth of a block to get. -1 get the last block.

#### Request

```java
curl --location --request POST 'https://bcn.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "get_block_header",
"params": {"height_or_depth": -1},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "block_header": {
            "already_generated_coins": 18444686308329240679,
            "already_generated_key_outputs": 63518445,
            "already_generated_transactions": 6665322,
            "base_reward": 7849781914,
            "binary_nonce": "a8c06108",
            "block_capacity_vote": 100000,
            "block_capacity_vote_median": 100000,
            "block_size": 624,
            "cumulative_difficulty": 30676358330795090,
            "difficulty": 10361588328,
            "effective_size_median": 0,
            "hash": "5099fb0b04b83d4f9862109608349d7a25938e6aa00b509366848589fe69917a",
            "height": 2403323,
            "major_version": 4,
            "minor_version": 7,
            "nonce": 140624040,
            "previous_block_hash": "bb98a8b435741b08a2d77700fc8c08a38d019f00bda4ccf96bee73965bac59ec",
            "reward": 7849781914,
            "size_median": 0,
            "timestamp": 1631799662,
            "timestamp_median": 1631795442,
            "transactions_fee": 0,
            "transactions_size": 269
        },
        "depth": -1,
        "orphan_status": false
    }
}
```
