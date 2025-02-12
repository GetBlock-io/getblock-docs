---
description: >-
  Example code for the eth_newPendingTransactionFilter json-rpc method. Сomplete
  guide on how to use eth_newPendingTransactionFilter json-rpc in GetBlock.io
  Web3 documentation.
---

# eth\_newPendingTransactionFilter - Huobi ECO Chain

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://heco.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_newPendingTransactionFilter",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x64833810630b210b680a897cd8dfe64a"
}
```
