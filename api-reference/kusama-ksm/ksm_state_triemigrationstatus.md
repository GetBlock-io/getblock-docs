---
description: >-
  Example code for the state_trieMigrationStatus  {disallowed} json-rpc method.
  Сomplete guide on how to use state_trieMigrationStatus  {disallowed} json-rpc
  in GetBlock.io Web3 documentation.
---

# state\_trieMigrationStatus {disallowed} - Kusama

#### Parameters

`at` - BlockHash

block hash

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "state_trieMigrationStatus",
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
