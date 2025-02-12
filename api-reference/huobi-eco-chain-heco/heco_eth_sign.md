---
description: >-
  Example code for the eth_sign json-rpc method. Сomplete guide on how to use
  eth_sign json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sign - Huobi ECO Chain

#### Parameters

`DATA` - string

address.

`DATA` - string

message to sign.

#### Request

```java
curl --location --request POST 'https://heco.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_sign",
"params": ["0xb0660a58f97733636b7555162b62e0e83786f79c", "0xdeadbeaf"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32000,
        "message": "unknown account"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
