---
description: >-
  Example code for the hard_fork_info json-rpc method. Сomplete guide on how to
  use hard_fork_info json-rpc in GetBlock.io Web3 documentation.
---

# hard\_fork\_info - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://xmr.getblock.io/mainnet/json_rpc' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "hard_fork_info",
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
        "earliest_height": 2210720,
        "enabled": true,
        "state": 1,
        "status": "OK",
        "threshold": 0,
        "top_hash": "",
        "untrusted": false,
        "version": 14,
        "votes": 10080,
        "voting": 14,
        "window": 10080
    }
}
```
