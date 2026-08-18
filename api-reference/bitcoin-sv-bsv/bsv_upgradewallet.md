---
description: >-
  Example code for the upgradewallet  {disallowed} json-rpc method. Сomplete
  guide on how to use upgradewallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# upgradewallet {disallowed} - Bitcoin SV

#### Parameters

`version` - numeric, optional, default=169900

The version number to upgrade to. Default is the latest wallet version.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "upgradewallet",
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
