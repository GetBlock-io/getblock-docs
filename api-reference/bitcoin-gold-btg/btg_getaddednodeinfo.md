---
description: >-
  Example code for the getaddednodeinfo  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddednodeinfo  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getaddednodeinfo {disallowed} - Bitcoin Gold

#### Parameters

`node` - string, optional, default=all nodes

If provided, return information about this specific node, otherwise all nodes are returned.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getaddednodeinfo",
"params": [null],
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
