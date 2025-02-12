---
description: >-
  Example code for the listlabels  {disallowed} json-rpc method. Сomplete guide
  on how to use listlabels  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listlabels {disallowed} - Bitcoin

#### Parameters

`purpose` - string, optional

Address purpose to list labels for (‘send’,’receive’). An empty string is the same as not providing this argument.

#### Request

```java
curl --location --request POST 'https://btc.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "listlabels",
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
