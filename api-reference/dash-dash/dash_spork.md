---
description: >-
  Example code for the spork json-rpc method. Сomplete guide on how to use spork
  json-rpc in GetBlock.io Web3 documentation.
---

# spork - Dash

#### Parameters

`mode` - string

The command to use

show - Display spork values

active - Display spork activation status

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "spork",
"params": ["show"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "SPORK_17_QUORUM_DKG_ENABLED": 0,
        "SPORK_19_CHAINLOCKS_ENABLED": 0,
        "SPORK_21_QUORUM_ALL_CONNECTED": 1,
        "SPORK_23_QUORUM_POSE": 0,
        "SPORK_2_INSTANTSEND_ENABLED": 0,
        "SPORK_3_INSTANTSEND_BLOCK_FILTERING": 0,
        "SPORK_9_SUPERBLOCKS_ENABLED": 0
    }
}
```
