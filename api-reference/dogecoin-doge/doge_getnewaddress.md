---
description: >-
  Example code for the getnewaddress  {disallowed} json-rpc method. Сomplete
  guide on how to use getnewaddress  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getnewaddress {disallowed} - Dogecoin

#### Parameters

`account` - string

If account is specified (recommended), it is added to the address book so that payments received with the address will be credited to it.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getnewaddress",
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
