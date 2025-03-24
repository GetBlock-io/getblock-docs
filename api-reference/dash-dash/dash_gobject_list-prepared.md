---
description: >-
  Example code for the gobject_list-prepared  {disallowed} json-rpc method.
  Сomplete guide on how to use gobject_list-prepared  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# gobject\_list-prepared {disallowed} - Dash

#### Parameters

`method name` - string

None

`count` - number (int)

Optional

Maximum number of objects to return.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "gobject",
"params": ["list-prepared", null],
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
