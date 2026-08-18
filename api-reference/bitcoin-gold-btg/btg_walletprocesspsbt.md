---
description: >-
  Example code for the walletprocesspsbt  {disallowed} json-rpc method. Сomplete
  guide on how to use walletprocesspsbt  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# walletprocesspsbt {disallowed} - Bitcoin Gold

#### Parameters

`psbt` - string, required

The transaction base64 string

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "walletprocesspsbt",
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
