---
description: >-
  Example code for the net_listening json-rpc method. Сomplete guide on how to
  use net_listening json-rpc in GetBlock.io Web3 documentation.
---

# net\_listening - Fuse Network

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://fuse.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "net_listening",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": true
}
```
