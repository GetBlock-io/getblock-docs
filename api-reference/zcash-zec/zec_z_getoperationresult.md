---
description: >-
  Example code for the z_getoperationresult  {disallowed} json-rpc method.
  Сomplete guide on how to use z_getoperationresult  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# z\_getoperationresult {disallowed} - Zcash

#### Parameters

`operationid` - array

Optional

A list of operation ids we are interested in. If not provided, examine all operations known to the node.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "z_getoperationresult",
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
