---
description: >-
  Example code for the get_connections json-rpc method. Сomplete guide on how to
  use get_connections json-rpc in GetBlock.io Web3 documentation.
---

# get\_connections - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "get_connections",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "connections": [
            {
                "address": "185.14.253.254:18080",
                "address_type": 1,
                "avg_download": 2,
                "avg_upload": 0,
                "connection_id": "d54c698669ca47438e59f050e58131a7",
                "current_download": 1,
                "current_upload": 0,
                "height": 2394582,
                "host": "185.14.253.254",
                "incoming": false,
                "ip": "185.14.253.254",
                "live_time": 6,
                "local_ip": false,
                "localhost": false,
                "peer_id": "714b4a329e3eca79",
                "port": "18080",
                "pruning_seed": 0,
                "recv_count": 15146,
                "recv_idle_time": 6,
                "rpc_credits_per_hash": 0,
                "rpc_port": 18089,
                "send_count": 400,
                "send_idle_time": 6,
                "state": "normal",
                "support_flags": 1
            }
        ],
        "status": "OK",
        "untrusted": false
    }
}
```
