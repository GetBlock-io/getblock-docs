---
description: >-
  Example code for the admin.setLogLevel  {disallowed} json-rpc method. Сomplete
  guide on how to use admin.setLogLevel  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# admin.setLogLevel {disallowed} - Avalanche

#### Parameters

`level` - string

level is the log level to be set.

#### Request

```java
curl --location --request POST 'https://avax.getblock.io/mainnet/ext/bc/C/rpc' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "admin.setLogLevel",
"params": ["info"],
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
