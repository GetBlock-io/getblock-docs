---
description: >-
  Example code for the get_statistics json-rpc method. Сomplete guide on how to
  use get_statistics json-rpc in GetBlock.io Web3 documentation.
---

# get\_statistics - Bitcoin Cash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "get_statistics",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "checkpoints": [
            {
                "counter": 503900,
                "hash": "fd0d60494bc00d75cf854932db0f6d69cfe495d474b5a50d79f57aa40fd68a97",
                "height": 2132145,
                "key_id": 0,
                "signature": "367c654feaf34ba50ca7f178433d74e9e894a6af6b39163f316d0a77b30a1602d7420b024ff2a2c3ab20b1d1ddc2cbddf97422ecde622dbf8f4c96234fd21d06"
            },
            {
                "counter": 796691,
                "hash": "5fdf6360439fe6fc1dfa5f1e95bafa60d8f66461834e349478af8fb6884d78fe",
                "height": 2403320,
                "key_id": 1,
                "signature": "6902c7cfb18dc75dd1e912ada594883f890299999a7be9e337d4fb0854af47043c720728e4435f10ebd86d82cd16a15b6762694be2b16d2aa1e1fa5d70e3310b"
            },
            {
                "counter": 804638,
                "hash": "5fdf6360439fe6fc1dfa5f1e95bafa60d8f66461834e349478af8fb6884d78fe",
                "height": 2403320,
                "key_id": 2,
                "signature": "fc93e75f9f76de36ca955753131d73202ae43563e1cd0737ae9442e00167ad09fed6d8aa07c00565bcc123c746ab10f3dfe54f4002ec26e441e01fed85657a06"
            }
        ],
        "connected_peers": [
            {
                "address": "45.77.7.147:8080",
                "is_incoming": false,
                "p2p_version": 4,
                "peer_id": 98147779483043330,
                "top_block_desc": {
                    "cumulative_difficulty": 0,
                    "hash": "5099fb0b04b83d4f9862109608349d7a25938e6aa00b509366848589fe69917a",
                    "height": 2403323
                }
            },
            {
                "address": "195.154.106.27:8080",
                "is_incoming": false,
                "p2p_version": 4,
                "peer_id": 13540377890649886956,
                "top_block_desc": {
                    "cumulative_difficulty": 0,
                    "hash": "5099fb0b04b83d4f9862109608349d7a25938e6aa00b509366848589fe69917a",
                    "height": 2403323
                }
            }
        ],
        "genesis_block_hash": "a742885cb01d11b7b36fb8bf14616d42cd3d8c1429a224df41afa81b86b8a3a8",
        "net": "main",
        "node_database_size": 34619015168,
        "peer_id": 12164859454942861108,
        "peer_list_gray": [],
        "peer_list_white": [],
        "platform": "linux",
        "start_time": 1630445391,
        "transaction_pool_count": 0,
        "transaction_pool_lowest_fee_per_byte": 0,
        "transaction_pool_max_size": 4000000,
        "transaction_pool_size": 0,
        "upgrade_decided_height": 0,
        "upgrade_votes_in_top_block": 0,
        "version": "v3.5.1"
    }
}
```
