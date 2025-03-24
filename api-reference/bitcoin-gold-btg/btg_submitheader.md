---
description: >-
  Example code for the submitheader  {disallowed} json-rpc method. Сomplete
  guide on how to use submitheader  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# submitheader {disallowed} - Bitcoin Gold

#### Parameters

`hexdata` - string, required

The hex-encoded block header data

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "submitheader",
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
