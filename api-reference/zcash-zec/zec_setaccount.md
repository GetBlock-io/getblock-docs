---
description: >-
  Example code for the setaccount  {disallowed} json-rpc method. Сomplete guide
  on how to use setaccount  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# setaccount {disallowed} - Zcash

#### Parameters

`zcashaddress` - string

The Zcash address to be associated with an account.

`account` - string

MUST be set to the empty string "" to represent the default account. Passing any other string will result in an error.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "setaccount",
"params": [null, null],
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
