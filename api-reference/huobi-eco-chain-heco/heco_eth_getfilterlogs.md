---
description: >-
  Example code for the eth_getFilterLogs json-rpc method. Сomplete guide on how
  to use eth_getFilterLogs json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getFilterLogs - Huobi ECO Chain

#### Parameters

`QUANTITY` - string

The filter id.

#### Request

```java
curl --location --request POST 'https://heco.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getFilterLogs",
"params": ["0x7373bf81a29e9491199289fab72c499f"],
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
