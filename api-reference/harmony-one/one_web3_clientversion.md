---
description: >-
  Example code for the web3_clientVersion json-rpc method. Сomplete guide on how
  to use web3_clientVersion json-rpc in GetBlock.io Web3 documentation.
---

# web3\_clientVersion - Harmony

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://one.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "web3_clientVersion",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "Harmony (C) 2023. harmony, version v7647-v2023.1.0-0-g09dba416 (runner@ 2023-01-26T10:25:00+0000)"
}
```
