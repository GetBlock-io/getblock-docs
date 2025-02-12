---
description: >-
  Example code for the get_last_block_header json-rpc method. Сomplete guide on
  how to use get_last_block_header json-rpc in GetBlock.io Web3 documentation.
---

# get\_last\_block\_header - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://xmr.getblock.io/mainnet/json_rpc' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "get_last_block_header",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "block_header": {
            "block_size": 156136,
            "block_weight": 156136,
            "cumulative_difficulty": 116197140699118346,
            "cumulative_difficulty_top64": 0,
            "depth": 0,
            "difficulty": 294928953975,
            "difficulty_top64": 0,
            "hash": "c5f1a60c9ce19bf08904f5ad338ce2d0b2a86e03911da37659ecb2e55d2d7768",
            "height": 2394581,
            "long_term_weight": 156136,
            "major_version": 14,
            "miner_tx_hash": "4dca37d67536963ed8c410fdb1a7f66c766b844d51d7cfdce7a791df4165a688",
            "minor_version": 14,
            "nonce": 1728,
            "num_txes": 87,
            "orphan_status": false,
            "pow_hash": "",
            "prev_hash": "82df904f4997886bc192a5e51b53b2a13a9319a6cb9a6d4be092704804cd2862",
            "reward": 992334385710,
            "timestamp": 1625067311,
            "wide_cumulative_difficulty": "0x19cd0af0f497b0a",
            "wide_difficulty": "0x44ab22b277"
        },
        "credits": 0,
        "status": "OK",
        "top_hash": "",
        "untrusted": false
    }
}
```
