---
description: >-
  Example code for the net_peerCount json-rpc method. Сomplete guide on how to
  use net_peerCount json-rpc in GetBlock.io Web3 documentation.
---

# net\_peerCount - Binance Smart Chain

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "net_peerCount",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x42"
}
```
