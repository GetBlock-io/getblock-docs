---
description: >-
  Example code for the theta.GetStatus json-rpc method. Сomplete guide on how to
  use theta.GetStatus json-rpc in GetBlock.io Web3 documentation.
---

# theta.GetStatus - Theta Network

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://theta.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "theta.GetStatus",
"params": {},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "address": "0xD572A7d644eFB4ac87C5Fd5e6c7692F2f391195b",
        "chain_id": "mainnet",
        "current_epoch": "11994516",
        "current_height": "11912628",
        "current_time": "1631111053",
        "genesis_block_hash": "0xd8836c6cf3c3ccea0b015b4ed0f9efb0ffe6254db793a515843c9d0f68cbab65",
        "latest_finalized_block_epoch": "11994514",
        "latest_finalized_block_hash": "0x6acc2c7dd0181f4eb0826526b73746297ceed58c6de2e4b146e572f5d20a91f7",
        "latest_finalized_block_height": "11912628",
        "latest_finalized_block_time": "1631111016",
        "peer_id": "0xD572A7d644eFB4ac87C5Fd5e6c7692F2f391195b",
        "syncing": false
    }
}
```
