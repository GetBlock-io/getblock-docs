---
description: >-
  Example code for the bls_generate  {disallowed} json-rpc method. Сomplete
  guide on how to use bls_generate  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# bls\_generate {disallowed} - Dash

#### Parameters

`submethod` - string

None

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "bls",
"params": ["generate"],
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
