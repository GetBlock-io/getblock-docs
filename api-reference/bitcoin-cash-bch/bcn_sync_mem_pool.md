---
description: >-
  Example code for the sync_mem_pool json-rpc method. Сomplete guide on how to
  use sync_mem_pool json-rpc in GetBlock.io Web3 documentation.
---

# sync\_mem\_pool - Bitcoin Cash

#### Parameters

`known_hashes` - array of string

Array of transactions in local memory pool. Should be sent sorted.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "sync_mem_pool",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "added_raw_transactions": [],
        "added_transactions": [],
        "removed_hashes": [],
        "status": {
            "incoming_peer_count": 0,
            "lower_level_error": "",
            "next_block_effective_median_size": 98872,
            "outgoing_peer_count": 8,
            "recommended_fee_per_byte": 100,
            "recommended_max_transaction_size": 98872,
            "top_block_cumulative_difficulty": 30676358330795090,
            "top_block_difficulty": 10361588328,
            "top_block_hash": "5099fb0b04b83d4f9862109608349d7a25938e6aa00b509366848589fe69917a",
            "top_block_height": 2403323,
            "top_block_timestamp": 1631799662,
            "top_block_timestamp_median": 1631795442,
            "top_known_block_height": 2403323,
            "transaction_pool_version": 2438
        }
    }
}
```
