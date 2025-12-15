---
description: >-
  Example code for the shh_addToGroup JSON RPC method. Сomplete guide on how to
  use shh_addToGroup JSON RPC in GetBlock Web3 documentation.
---

# shh\_addToGroup {disallowed} - Arbitrum

#### Parameters

`address` - data

The identity address to add to a group

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "shh_addToGroup",
  "params": [null],
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
