---
description: >-
  Example code for the shh_getMessages  {disallowed} json-rpc method. Сomplete
  guide on how to use shh_getMessages  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# shh\_getMessages {disallowed} - Arbitrum

#### Parameters

`id` - string

the filter id.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "shh_getMessages",
  "params": [null],
  "id": "getblock.io"
}'
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
