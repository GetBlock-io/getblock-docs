---
description: >-
  Example code for the shh_newFilter  {disallowed} json-rpc method. Сomplete
  guide on how to use shh_newFilter  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# shh\_newFilter {disallowed} - Optimism

#### Parameters

`filters` - json object

filter object: { "to": "address" (string, optional) - Identity of the receiver. When present it will try to decrypt any incoming message if the client holds the private key to this identity. "topics": \["topic"] (array of string) - Array of topics which the incoming message’s topics should match. }

#### Request

```java
curl --location --request POST 'https://optimism.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "shh_newFilter",
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
