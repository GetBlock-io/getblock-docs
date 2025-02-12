---
description: >-
  Example code for the mnsync json-rpc method. Сomplete guide on how to use
  mnsync json-rpc in GetBlock.io Web3 documentation.
---

# mnsync - Dash

#### Parameters

`mode` - string

The command mode to use:

status - Get masternode sync status

next - Move to next sync asset

reset - Reset sync status

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "mnsync",
"params": ["status"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "AssetID": 999,
        "AssetName": "MASTERNODE_SYNC_FINISHED",
        "AssetStartTime": 1631264662,
        "Attempt": 0,
        "IsBlockchainSynced": true,
        "IsSynced": true
    }
}
```
