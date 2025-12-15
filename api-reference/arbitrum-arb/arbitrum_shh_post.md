---
description: >-
  Example code for the shh_post JSON RPC method. Сomplete guide on how to use
  shh_post  JSON RPC in GetBlock Web3 documentation.
---

# shh\_post {disallowed} - Arbitrum

#### Parameters

`wisper` - wisper object

wisper post object

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "shh_post",
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
