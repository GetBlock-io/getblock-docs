---
description: >-
  Example code for the system_name  {disallowed} json-rpc method. Сomplete guide
  on how to use system_name  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# system\_name {disallowed} - Kusama

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://ksm.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "system_name",
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
