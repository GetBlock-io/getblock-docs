---
description: >-
  Example code for the net_version json-rpc method. Сomplete guide on how to use
  net_version json-rpc in GetBlock.io Web3 documentation.
---

# net\_version - Gnosis

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://gno.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "net_version",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "100"
}
```
