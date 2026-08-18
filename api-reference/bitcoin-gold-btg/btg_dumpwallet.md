---
description: >-
  Example code for the dumpwallet  {disallowed} json-rpc method. Сomplete guide
  on how to use dumpwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# dumpwallet {disallowed} - Bitcoin Gold

#### Parameters

`filename` - None

string, required

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "dumpwallet",
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
