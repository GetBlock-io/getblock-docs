---
description: >-
  Example code for the getreceivedbylabel  {disallowed} json-rpc method.
  Сomplete guide on how to use getreceivedbylabel  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getreceivedbylabel {disallowed} - DigiByte

#### Parameters

`label` - string, required

The selected label, may be the default label using “”.

`minconf` - numeric, optional, default=1

Only include transactions confirmed at least this many times.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getreceivedbylabel",
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
