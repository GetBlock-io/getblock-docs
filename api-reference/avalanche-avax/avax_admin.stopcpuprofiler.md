---
description: >-
  Example code for the admin.stopCPUProfiler  {disallowed} json-rpc method.
  Сomplete guide on how to use admin.stopCPUProfiler  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# admin.stopCPUProfiler {disallowed} - Avalanche

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' \
--data-raw '{
  "jsonrpc": "2.0",
  "method": "admin.stopCPUProfiler",
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
