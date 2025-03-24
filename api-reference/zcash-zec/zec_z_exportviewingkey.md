---
description: >-
  Example code for the z_exportviewingkey  {disallowed} json-rpc method.
  Сomplete guide on how to use z_exportviewingkey  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# z\_exportviewingkey {disallowed} - Zcash

#### Parameters

`zaddr` - string

The zaddr for the private key.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_exportviewingkey",
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
