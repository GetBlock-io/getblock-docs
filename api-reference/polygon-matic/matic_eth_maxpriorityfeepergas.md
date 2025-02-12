---
description: >-
  Example code for the eth_maxPriorityFeePerGas json-rpc method. Сomplete guide
  on how to use eth_maxPriorityFeePerGas json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_maxPriorityFeePerGas - Polygon

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://matic.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_maxPriorityFeePerGas",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x6eba25eb8"
}
```
