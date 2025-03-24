---
description: >-
  Example code for the get_fee_estimate json-rpc method. Сomplete guide on how
  to use get_fee_estimate json-rpc in GetBlock.io Web3 documentation.
---

# get\_fee\_estimate - Monero

#### Parameters

`grace_blocks` - unsigned int

Optional

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "get_fee_estimate",
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
        "fee": 6407,
        "quantization_mask": 10000,
        "status": "OK",
        "top_hash": "",
        "untrusted": false
    }
}
```
