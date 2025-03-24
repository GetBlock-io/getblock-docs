---
description: >-
  Example code for the getchaintxstats json-rpc method. Сomplete guide on how to
  use getchaintxstats json-rpc in GetBlock.io Web3 documentation.
---

# getchaintxstats - Dash

#### Parameters

`nblocks` - numeric

Optional.

Size of the window in number of blocks (default: one month).

`blockhash` - string

Optional.

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
        "time": 1631268126,
        "txcount": 38626270,
        "txrate": 0.1339127529836747,
        "window_block_count": 17280,
        "window_final_block_hash": "0000000000000019cd3a50eedc418372f7aa9060d6ded56a769a51a93dfe0e4a",
        "window_interval": 2723990,
        "window_tx_count": 364777
    }
}
```
