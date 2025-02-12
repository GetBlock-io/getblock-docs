---
description: >-
  Example code for the eth_coinbase json-rpc method. Сomplete guide on how to
  use eth_coinbase json-rpc in GetBlock.io Web3 documentation.
---

# eth\_coinbase - Moonbeam

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://glmr.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_coinbase",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xecca07badbd38937122b82ec8afcf86b1e2b7939"
}
```
