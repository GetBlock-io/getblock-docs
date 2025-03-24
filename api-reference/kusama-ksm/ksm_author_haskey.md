---
description: >-
  Example code for the author_hasKey  {disallowed} json-rpc method. Сomplete
  guide on how to use author_hasKey  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# author\_hasKey {disallowed} - Kusama

#### Parameters

`publicKey` - Bytes

public key in bytes

`keyType` - Text

key type

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "author_hasKey",
"params": ["publickeyinbytes", "keytype"],
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
