---
description: >-
  Example code for the stop  {disallowed} json-rpc method. Сomplete guide on how
  to use stop  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# stop {disallowed} - Bitcoin

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "stop",
"params": [],
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
