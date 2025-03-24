---
description: >-
  Example code for the EXPERIMENTAL_genesis_config  {disallowed} json-rpc
  method. Сomplete guide on how to use EXPERIMENTAL_genesis_config  {disallowed}
  json-rpc in GetBlock.io Web3 documentation.
---

# EXPERIMENTAL\_genesis\_config {disallowed} - NEAR Protocol

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "EXPERIMENTAL_genesis_config",
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
