---
description: >-
  Example code for the getaddressesbylabel  {disallowed} json-rpc method.
  Сomplete guide on how to use getaddressesbylabel  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getaddressesbylabel {disallowed} - Dash

#### Parameters

`Label` - string

The name of the label associated with the addresses to get. To get addresses from the default account, pass an empty string ("").

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getaddressesbylabel",
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
