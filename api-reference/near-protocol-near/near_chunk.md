---
description: >-
  Example code for the chunk json-rpc method. Сomplete guide on how to use chunk
  json-rpc in GetBlock.io Web3 documentation.
---

# chunk - NEAR Protocol

#### Parameters

`id` - map

One of: - chunk\_id - chunk id hash - - block\_id - can be either block number or block hash - shard\_id - shard id

#### Request

```java
curl --location --request POST 'https://near.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "chunk",
"params": {"block_id": 58934027, "shard_id": 0},
"id": "getblock.io"}'
```

#### Response

```java
{
    "jsonrpc": "2.0",
    "id": "getblock.io",
    "result": {
        "author": "bitcat.pool.f863973.m0",
        "header": {
            "chunk_hash": "EBM2qg5cGr47EjMPtH88uvmXHDHqmWPzKaQadbWhdw22",
            "prev_block_hash": "2yUTTubrv1gJhTUVnHXh66JG3qxStBqySoN6wzRzgdVD",
            "outcome_root": "11111111111111111111111111111111",
            "prev_state_root": "HqWDq3f5HJuWnsTfwZS6jdAUqDjGFSTvjhb846vV27dx",
            "encoded_merkle_root": "9zYue7drR1rhfzEEoc4WUXzaYRnRNihvRoGt1BgK7Lkk",
            "encoded_length": 8,
            "height_created": 17821130,
            "height_included": 17821130,
            "shard_id": 0,
            "gas_used": 0,
            "gas_limit": 1000000000000000,
            "rent_paid": "0",
            "validator_reward": "0",
            "balance_burnt": "0",
            "outgoing_receipts_root": "H4Rd6SGeEBTbxkitsCdzfu9xL9HtZ2eHoPCQXUeZ6bW4",
            "tx_root": "11111111111111111111111111111111",
            "validator_proposals": [],
            "signature": "ed25519:4iPgpYAcPztAvnRHjfpegN37Rd8dTJKCjSd1gKAPLDaLcHUySJHjexMSSfC5iJVy28vqF9VB4psz13x2nt92cbR7"
        },
        "transactions": [],
        "receipts": []
    }
}
```
