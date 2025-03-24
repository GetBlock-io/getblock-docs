---
description: >-
  Example code for the thetacli.ListKeys  {disallowed} json-rpc method. Сomplete
  guide on how to use thetacli.ListKeys  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# thetacli.ListKeys {disallowed} - Theta Network

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "thetacli.ListKeys",
"params": {},
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
