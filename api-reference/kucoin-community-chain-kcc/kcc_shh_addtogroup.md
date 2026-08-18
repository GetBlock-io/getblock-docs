---
description: >-
  Example code for the shh_addToGroup  {disallowed} json-rpc method. Сomplete
  guide on how to use shh_addToGroup  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# shh\_addToGroup {disallowed} - KuCoin Community Chain

#### Parameters

`address` - data

The identity address to add to a group

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "shh_addToGroup",
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
