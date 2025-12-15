---
description: >-
  Example code for the shh_version JSON RPC method. Сomplete guide on how to use
  shh_version JSON RPC in GetBlock Web3 documentation.
---

# shh\_version {disallowed} - Arbitrum

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "shh_version",
  "params": [],
  "id": "getblock.io"
}'
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
