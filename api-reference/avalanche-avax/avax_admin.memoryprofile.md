---
description: >-
  Example code for the admin.memoryProfile  {disallowed} json-rpc method.
  Сomplete guide on how to use admin.memoryProfile  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# admin.memoryProfile {disallowed} - Avalanche

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "admin.memoryProfile",
  "params": [],
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
