---
description: >-
  Example code for the ftm_currentEpoch json-rpc method. Сomplete guide on how
  to use ftm_currentEpoch json-rpc in GetBlock.io Web3 documentation.
---

# ftm\_currentEpoch - Fantom

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://ftm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "ftm_currentEpoch",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "0x34398"
}
```
