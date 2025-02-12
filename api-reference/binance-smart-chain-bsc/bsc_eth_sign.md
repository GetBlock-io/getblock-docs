---
description: >-
  Example code for the eth_sign json-rpc method. Сomplete guide on how to use
  eth_sign json-rpc in GetBlock.io Web3 documentation.
---

# eth\_sign - Binance Smart Chain

#### Parameters

`DATA` - hex string

20 Bytes - address

`DATA` - hex string

N Bytes - message to sign

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_sign",
"params": ["0xcee8ae756461e2653b88aefdbd70c1144de52b23", "0xbcda"],
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
