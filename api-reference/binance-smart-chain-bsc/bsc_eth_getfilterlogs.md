---
description: >-
  Example code for the eth_getFilterLogs json-rpc method. Сomplete guide on how
  to use eth_getFilterLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterLogs - Binance Smart Chain

#### Parameters

`data` - None

Filter ID.

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getFilterLogs",
"params": ["0x5ace5de3985749b6a1b2b0d3f3e1fb69"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32000,
        "message": "filter not found"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
