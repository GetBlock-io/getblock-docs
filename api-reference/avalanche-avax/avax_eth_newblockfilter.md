---
description: >-
  Example code for the eth_newBlockFilter json-rpc method. Сomplete guide on how
  to use eth_newBlockFilter json-rpc in GetBlock.io Web3 documentation.
---

# eth\_newBlockFilter - Avalanche

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://avax.getblock.io/mainnet/ext/bc/C/rpc' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_newBlockFilter",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0xde660a80c2931ac342ec8c086baa2f09"
}
```
