---
description: >-
  Example code for the getconnectioncount  {disallowed} json-rpc method.
  Сomplete guide on how to use getconnectioncount  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getconnectioncount {disallowed} - DigiByte

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dgb.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getconnectioncount",
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
