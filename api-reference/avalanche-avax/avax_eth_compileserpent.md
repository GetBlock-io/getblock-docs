---
description: >-
  Example code for the eth_compileSerpent  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_compileSerpent  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_compileSerpent {disallowed} - Avalanche

#### Parameters

`code` - string

The source code.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \ 
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "eth_compileSerpent",
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
