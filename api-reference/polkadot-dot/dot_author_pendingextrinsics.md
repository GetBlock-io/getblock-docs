---
description: >-
  Example code for the author_pendingExtrinsics  {disallowed} json-rpc method.
  Сomplete guide on how to use author_pendingExtrinsics  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# author\_pendingExtrinsics {disallowed} - Polkadot

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "author_pendingExtrinsics",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": "null",
    "id": "getblock.io",
    "status_code": 405,
    "message": "Method not allowed"
}
```
