---
description: >-
  Example code for the protx_diff  {disallowed} json-rpc method. Сomplete guide
  on how to use protx_diff  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# protx\_diff {disallowed} - Dash

#### Parameters

`submethod` - string

None

`baseBlock` - number (int)

Start block height.

`block` - bool

End block height.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "protx",
"params": ["diff", null, null],
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
