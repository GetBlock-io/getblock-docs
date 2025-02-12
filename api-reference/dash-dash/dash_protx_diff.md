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
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
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
