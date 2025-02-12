---
description: >-
  Example code for the getindexinfo json-rpc method. Сomplete guide on how to
  use getindexinfo json-rpc in GetBlock.io Web3 documentation.
---

# getindexinfo - DigiByte

#### Parameters

`index_name` - string, optional

Filter results for an index with a specific name.

#### Request

```java
curl --location --request POST 'https://dgb.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getindexinfo",
"params": [null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "txindex": {
            "synced": true,
            "best_block_height": 680586
        },
        "basic block filter index": {
            "synced": true,
            "best_block_height": 680586
        }
    }
}
```
