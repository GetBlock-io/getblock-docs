---
description: >-
  Example code for the getdifficulty json-rpc method. Сomplete guide on how to
  use getdifficulty json-rpc in GetBlock.io Web3 documentation.
---

# getdifficulty - Dash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getdifficulty",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": null,
    "id": "getblock.io",
    "result": 103704145.924301
}
```
