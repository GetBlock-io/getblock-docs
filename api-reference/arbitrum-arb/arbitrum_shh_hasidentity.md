---
description: >-
  Example code for the shh_hasIdentity  {disallowed} json-rpc method. Сomplete
  guide on how to use shh_hasIdentity  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# shh\_hasIdentity {disallowed} - Arbitrum

#### Parameters

`address` - data

the address of the new identiy.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/arbitrum/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "shh_hasIdentity",
  "params": [null],
  "id": "getblock.io"
}'
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
