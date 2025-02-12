---
description: >-
  Example code for the get_info json-rpc method. Сomplete guide on how to use
  get_info json-rpc in GetBlock.io Web3 documentation.
---

# get\_info - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://xmr.getblock.io/mainnet/json_rpc' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "get_info",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "adjusted_time": 1625067431,
        "alt_blocks_count": 158,
        "block_size_limit": 600000,
        "block_size_median": 300000,
        "block_weight_limit": 600000,
        "block_weight_median": 300000,
        "bootstrap_daemon_address": "",
        "busy_syncing": false,
        "credits": 0,
        "cumulative_difficulty": 116197140699118346,
        "cumulative_difficulty_top64": 0,
        "database_size": 114352345088,
        "difficulty": 294558564173,
        "difficulty_top64": 0,
        "free_space": 514427379712,
        "grey_peerlist_size": 4999,
        "height": 2394582,
        "height_without_bootstrap": 2394582,
        "incoming_connections_count": 0,
        "mainnet": true,
        "nettype": "mainnet",
        "offline": false,
        "outgoing_connections_count": 12,
        "rpc_connections_count": 7,
        "stagenet": false,
        "start_time": 1614267442,
        "status": "OK",
        "synchronized": true,
        "target": 120,
        "target_height": 0,
        "testnet": false,
        "top_block_hash": "c5f1a60c9ce19bf08904f5ad338ce2d0b2a86e03911da37659ecb2e55d2d7768",
        "top_hash": "",
        "tx_count": 14490301,
        "tx_pool_size": 13,
        "untrusted": false,
        "update_available": true,
        "version": "0.17.0.0-b8f3e44a3",
        "was_bootstrap_ever_used": false,
        "white_peerlist_size": 1000,
        "wide_cumulative_difficulty": "0x19cd0af0f497b0a",
        "wide_difficulty": "0x44950eff4d"
    }
}
```
