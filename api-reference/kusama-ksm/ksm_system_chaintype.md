---
description: >-
  Example code for the system_chainType json-rpc method. Сomplete guide on how
  to use system_chainType json-rpc in GetBlock.io Web3 documentation.
---

# system\_chainType - Kusama

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "system_chainType",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "Live"
}
```
