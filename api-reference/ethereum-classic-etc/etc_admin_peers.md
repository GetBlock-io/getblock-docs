---
description: >-
  Example code for the admin_peers  {disallowed} json-rpc method. Сomplete guide
  on how to use admin_peers  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# admin\_peers {disallowed} - Ethereum Classic

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "admin_peers",
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
