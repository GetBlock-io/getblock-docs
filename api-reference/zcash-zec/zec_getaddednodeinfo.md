---
description: >-
  Example code for the getaddednodeinfo  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddednodeinfo  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# getaddednodeinfo {disallowed} - Zcash

#### Parameters

`dns` - boolean

If false, only a list of added nodes will be provided, otherwise connected information will also be available.

`node` - string

Optional.

If provided, return information about this specific node, otherwise all nodes are returned.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getaddednodeinfo",
"params": [null, null],
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
