---
description: >-
  Example code for the setcoinjoinrounds  {disallowed} json-rpc method. Сomplete
  guide on how to use setcoinjoinrounds  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# setcoinjoinrounds {disallowed} - Dash

#### Parameters

`Rounds` - int

The number of rounds to use (minimum: 1, maximum: 16)

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "setcoinjoinrounds",
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
