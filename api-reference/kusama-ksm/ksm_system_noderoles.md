---
description: >-
  Example code for the system_nodeRoles  {disallowed} json-rpc method. Сomplete
  guide on how to use system_nodeRoles  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# system\_nodeRoles {disallowed} - Kusama

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "system_nodeRoles",
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
