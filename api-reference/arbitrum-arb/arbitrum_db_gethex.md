---
description: >-
  Example code for the db_getHex JSON RPC method. Сomplete guide on how to use
  db_getHex  JSON RPC in GetBlock Web3 documentation.
---

# db\_getHex {disallowed} - Arbitrum

#### Parameters

`database` - string

Database name

`key` - string

key name

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0", "method": "db_getHex", "params": [null, null], "id": "getblock.io"}'
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
