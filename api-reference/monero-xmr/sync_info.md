---
description: >-
  Example code for the sync_info json-rpc method. Сomplete guide on how to use
  sync_info json-rpc in GetBlock.io Web3 documentation.
---

# sync\_info - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "sync_info",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "credits": 0,
        "height": 2394582,
        "next_needed_pruning_seed": 0,
        "overview": "[]",
        "peers": [
            {
                "info": {
                    "address": "185.14.253.254:18080",
                    "address_type": 1,
                    "avg_download": 1,
                    "avg_upload": 0,
                    "connection_id": "d54c698669ca47438e59f050e58131a7",
                    "current_download": 1,
                    "current_upload": 1,
                    "height": 2394582,
                    "host": "185.14.253.254",
                    "incoming": false,
                    "ip": "185.14.253.254",
                    "live_time": 24,
                    "local_ip": false,
                    "localhost": false,
                    "peer_id": "714b4a329e3eca79",
                    "port": "18080",
                    "pruning_seed": 0,
                    "recv_count": 31516,
                    "recv_idle_time": 1,
                    "rpc_credits_per_hash": 0,
                    "rpc_port": 18089,
                    "send_count": 20685,
                    "send_idle_time": 1,
                    "state": "normal",
                    "support_flags": 1
                }
            }
        ],
        "status": "OK",
        "target_height": 0,
        "top_hash": "",
        "untrusted": false
    }
}
```
