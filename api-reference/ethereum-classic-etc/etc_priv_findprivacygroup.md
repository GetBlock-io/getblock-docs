---
description: >-
  Example code for the priv_findPrivacyGroup  {disallowed} json-rpc method.
  Сomplete guide on how to use priv_findPrivacyGroup  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# priv\_findPrivacyGroup {disallowed} - Ethereum Classic

#### Parameters

`array of data` - None

Members specified by Orion public keys.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "priv_findPrivacyGroup",
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
