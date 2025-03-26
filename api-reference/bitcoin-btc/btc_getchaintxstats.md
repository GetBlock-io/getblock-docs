---
description: >-
  Example code for the getchaintxstats json-rpc method. Сomplete guide on how to
  use getchaintxstats json-rpc in GetBlock.io Web3 documentation.
---

# getchaintxstats - Bitcoin

#### Parameters

`nblocks` - numeric, optional, default=one month

Size of the window in number of blocks

`blockhash` - string, optional, default=chain tip

The hash of the block that ends the window.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getchaintxstats",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": {
        "time": 1619167479,
        "txcount": 325942210,
        "txrate": 3.981989438565958,
        "window_block_count": 4320,
        "window_final_block_hash": "0000000000000000023a561e1ea370153aac5d1504726d1a039032831c05fcfc",
        "window_interval": 2469551,
        "window_tx_count": 9833726
    }
}
```
