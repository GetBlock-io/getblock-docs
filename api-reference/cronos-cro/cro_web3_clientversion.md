---
description: >-
  Example code for the web3_clientVersion json-rpc method. Сomplete guide on how
  to use web3_clientVersion json-rpc in GetBlock.io Web3 documentation.
---

# web3\_clientVersion - Cronos

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' 
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
    "result": "Version dev ()\nCompiled at  using Go go1.19.6 (amd64)"
}
```
