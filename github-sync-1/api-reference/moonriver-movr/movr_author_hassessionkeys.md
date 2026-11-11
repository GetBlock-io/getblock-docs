---
description: >-
  Example code for the author_hasSessionKeys  {disallowed} json-rpc method.
  Сomplete guide on how to use author_hasSessionKeys  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# author\_hasSessionKeys {disallowed} - Moonriver

#### Parameters

`sessionKeys` - Bytes

session keys in bytes

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "author_hasSessionKeys",
"params": ["sessionkeysinbytes"],
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
