---
description: >-
  Example code for the priv_createPrivacyGroup  {disallowed} json-rpc method.
  Сomplete guide on how to use priv_createPrivacyGroup  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# priv\_createPrivacyGroup {disallowed} - Ethereum Classic

#### Parameters

`Object` - None

Request options

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "priv_createPrivacyGroup",
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
