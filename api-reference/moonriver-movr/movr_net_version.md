---
description: >-
  Example code for the net_version  {disallowed} json-rpc method. Сomplete guide
  on how to use net_version  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# net\_version {disallowed} - Moonriver

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "net_version",
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
