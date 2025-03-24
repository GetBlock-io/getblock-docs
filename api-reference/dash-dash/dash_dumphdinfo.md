---
description: >-
  Example code for the dumphdinfo  {disallowed} json-rpc method. Сomplete guide
  on how to use dumphdinfo  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# dumphdinfo {disallowed} - Dash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "dumphdinfo",
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
