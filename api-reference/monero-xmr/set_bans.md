---
description: >-
  Example code for the set_bans  {disallowed} json-rpc method. Сomplete guide on
  how to use set_bans  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# set\_bans {disallowed} - Monero

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://xmr.getblock.io/mainnet/json_rpc' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "set_bans",
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
