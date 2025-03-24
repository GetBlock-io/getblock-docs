---
description: >-
  Example code for the priv_getPrivacyPrecompileAddress  {disallowed} json-rpc
  method. Сomplete guide on how to use priv_getPrivacyPrecompileAddress 
  {disallowed} json-rpc in GetBlock.io Web3 documentat
---

# priv\_getPrivacyPrecompileAddress {disallowed} - Ethereum Classic

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "priv_getPrivacyPrecompileAddress",
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
