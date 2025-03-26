---
description: >-
  Example code for the web3_clientVersion json-rpc method. Сomplete guide on how
  to use web3_clientVersion json-rpc in GetBlock.io Web3 documentation.
---

# web3\_clientVersion - Avalanche

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
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
    "result": "v0.12.0"
}
```
