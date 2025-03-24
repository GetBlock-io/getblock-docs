---
description: >-
  Example code for the state_subscribeRuntimeVersion  {disallowed} json-rpc
  method. Сomplete guide on how to use state_subscribeRuntimeVersion 
  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# state\_subscribeRuntimeVersion {disallowed} - Polkadot

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "state_subscribeRuntimeVersion",
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
