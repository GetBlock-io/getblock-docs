---
description: >-
  Example code for the clearbanned  {disallowed} json-rpc method. Сomplete guide
  on how to use clearbanned  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# clearbanned {disallowed} - Bitcoin SV

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "clearbanned",
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
