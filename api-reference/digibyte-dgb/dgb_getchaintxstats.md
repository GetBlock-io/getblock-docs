---
description: >-
  Example code for the getchaintxstats json-rpc method. Сomplete guide on how to
  use getchaintxstats json-rpc in GetBlock.io Web3 documentation.
---

# getchaintxstats - DigiByte

#### Parameters

`nblocks` - numeric, optional, default=one month

Size of the window in number of blocks

`blockhash` - string, optional, default=chain tip

The hash of the block that ends the window.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
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
        "time": 1631538193,
        "txcount": 35739265,
        "txrate": 0.1396385228526447,
        "window_block_count": 172800,
        "window_final_block_hash": "b034141fc8e8024752b39513c9ed6db14690bf3a7fe9911a89c83578a73f0457",
        "window_interval": 2581408,
        "window_tx_count": 360464
    }
}
```
