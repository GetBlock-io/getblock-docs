---
description: >-
  Example code for the status json-rpc method. Сomplete guide on how to use
  status json-rpc in GetBlock.io Web3 documentation.
---

# status - NEAR Protocol

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://near.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "status",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "chain_id": "mainnet",
        "latest_protocol_version": 51,
        "protocol_version": 51,
        "rpc_addr": "0.0.0.0:3030",
        "sync_info": {
            "earliest_block_hash": "FD1M8N8Jx4cH79gLCG9ZaZKjqANmhnSkecMu54jUPf1r",
            "earliest_block_height": 60858604,
            "earliest_block_time": "2022-03-06T08:24:30.498760788Z",
            "latest_block_hash": "Q6rkJMtRRUQQfnRGRCffbGTaTLmfFRHzPTXLAXQPTq5",
            "latest_block_height": 61069216,
            "latest_block_time": "2022-03-09T10:02:27.129971340Z",
            "latest_state_root": "HZZmyq27AAzqxasSSfefD7eggXiYErj23KWQQKj2oYVw",
            "syncing": false
        },
        "validator_account_id": null,
        "validators": [
            {
                "account_id": "staked.poolv1.near",
                "is_slashed": false
            },
            {
                "account_id": "bzam6yjpnfnxsdmjf6pw.poolv1.near",
                "is_slashed": false
            },
            {
                "account_id": "aurora.pool.near",
                "is_slashed": false
            },
            {
                "account_id": "astro-stakers.poolv1.near",
                "is_slashed": false
            },
            {
                "account_id": "bisontrails.poolv1.near",
                "is_slashed": false
            },
            {
                "account_id": "dragonfly.poolv1.near",
                "is_slashed": false
            }
        ],
        "version": {
            "build": "crates-0.11.0-80-g164991d7a",
            "version": "1.24.0"
        }
    }
}
```
