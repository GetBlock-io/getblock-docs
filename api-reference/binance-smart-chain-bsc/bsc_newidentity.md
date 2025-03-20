---
description: >-
  Example code for the newIdentity  {disallowed} json-rpc method. Сomplete guide
  on how to use newIdentity  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# newIdentity {disallowed} - Binance Smart Chain

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "newIdentity",
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
