---
description: >-
  Example code for the gobject_prepare  {disallowed} json-rpc method. Сomplete
  guide on how to use gobject_prepare  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# gobject\_prepare {disallowed} - Dash

#### Parameters

`method name` - string

None

`parent-hash` - string (hex)

Hash of the parent object. Usually the root node which has a hash of 0

`revision` - int

Object revision number

`time` - int\_64t

Create time (Unix epoch time)

`data-hex` - string (hex)

Updated in Dash Core 0.14.0 to require all new proposals to use JSON serialization.

Object data (JSON object with governance details).

`use-IS` - boolean

Optional

Deprecated and ignored since Dash Core 0.15.0.

`outputHash` - string (hex)

Optional

The single output to submit the proposal fee from.

Added in Dash Core 0.13.0

`outputIndex` - numeric

Optional

The output index (required if the outputHash parameter is provided)

Added in Dash Core 0.13.0

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "gobject",
"params": ["prepare", null, null, null, null, null, null, null],
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
