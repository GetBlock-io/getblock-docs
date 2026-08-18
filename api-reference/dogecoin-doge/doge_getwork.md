---
description: >-
  Example code for the getwork  {disallowed} json-rpc method. Сomplete guide on
  how to use getwork  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# getwork {disallowed} - Dogecoin

#### Parameters

`data` - string

Optional.

Result from remote mining.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getwork",
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
