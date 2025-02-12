---
description: >-
  Example code for the system_localPeerId  {disallowed} json-rpc method.
  Сomplete guide on how to use system_localPeerId  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# system\_localPeerId {disallowed} - Polkadot

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://dot.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "system_localPeerId",
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
