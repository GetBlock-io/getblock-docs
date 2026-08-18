---
description: >-
  Example code for the net_peerCount JSON RPC method. Сomplete guide on how to
  use net_peerCount  JSON RPC in GetBlock Web3 documentation.
---

# net\_peerCount {disallowed} - Arbitrum

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "net_peerCount",
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
