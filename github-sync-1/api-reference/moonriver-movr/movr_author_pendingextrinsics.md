---
description: >-
  Example code for the author_pendingExtrinsics  {disallowed} json-rpc method.
  Сomplete guide on how to use author_pendingExtrinsics  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# author\_pendingExtrinsics {disallowed} - Moonriver

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
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
