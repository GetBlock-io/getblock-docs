---
description: >-
  Example code for the network_info json-rpc method. Сomplete guide on how to
  use network_info json-rpc in GetBlock.io Web3 documentation.
---

# network\_info - NEAR Protocol

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://near.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "network_info",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "active_peers": [
            {
                "account_id": null,
                "addr": "34.91.52.248:24567",
                "id": "ed25519:8MvmovbbJ4PYNgMCpBwvQRA2P4ZbpEcZGHX1448G3Ehm"
            },
            {
                "account_id": null,
                "addr": "54.179.201.180:24567",
                "id": "ed25519:7rN5RBbgEu6c778zdX4aEqWCofgjzSC9wYzi61Pcr2AL"
            },
            {
                "account_id": null,
                "addr": "185.209.177.15:24567",
                "id": "ed25519:8rhSRLCrzPCS4JsgHFadgWGKvKCPCweq5oH3eL6iH6SK"
            }
        ],
        "known_producers": [
            {
                "account_id": "chelovek_iz_naroda.poolv1.near",
                "addr": null,
                "peer_id": "ed25519:GyuMR3KYDMVQZVH87aZqHSRgqsD1ZTsRMN8JZj1gCa2P"
            },
            {
                "account_id": "incrypted.poolv1.near",
                "addr": null,
                "peer_id": "ed25519:Hp74AobcFkyKqDU7NZQAhqbvHTesouRCveCFWfEe6NAp"
            },
            {
                "account_id": "dexagon.poolv1.near",
                "addr": null,
                "peer_id": "ed25519:6v3juNRvjGSDoauKNzQJunHv2ktPdEMj5s6zoZ6NEnqJ"
            }
        ],
        "num_active_peers": 30,
        "peer_max_count": 40,
        "received_bytes_per_sec": 575716,
        "sent_bytes_per_sec": 505720
    }
}
```
