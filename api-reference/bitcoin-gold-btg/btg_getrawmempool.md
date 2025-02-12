---
description: >-
  Example code for the getrawmempool json-rpc method. Сomplete guide on how to
  use getrawmempool json-rpc in GetBlock.io Web3 documentation.
---

# getrawmempool - Bitcoin Gold

#### Parameters

`verbose` - boolean, optional, default=false

True for a json object, false for array of transaction ids

`mempool_sequence` - boolean, optional, default=false

If verbose=false, returns a json object with transaction list and mempool sequence number attached.

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getrawmempool",
"params": [null, null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": [
        "d7d42e8c0ee7721dc5e417fb746c4ac404c7f2cd39e1343b3aa03741fa777782",
        "27b8644cb6d365e9f57a40d01ce3599d1cfb4f50ffdfaf927bcd1277c96dae12",
        "fc5e53103d5c9d83e76d8f7f133fb2a519efa572e451392e73a90629895f89f3",
        "442723267ed7f1b39b66316c8144aef9f3a8fcf30d53ff6750ed34f69e11aa9a"
    ]
}
```
