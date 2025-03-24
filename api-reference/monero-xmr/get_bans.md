---
description: >-
  Example code for the get_bans  {disallowed} json-rpc method. Сomplete guide on
  how to use get_bans  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# get\_bans {disallowed} - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "get_bans",
"params": {},
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
