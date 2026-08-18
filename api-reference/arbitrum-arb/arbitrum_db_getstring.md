---
description: >-
  Example code for the db_getString  JSON RPC method. Сomplete guide on how to
  use db_getString JSON RPC in GetBlock Web3 documentation.
---

# db\_getString {disallowed} - Arbitrum

#### Parameters

`database` - string

database name

`key` - string

key name

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0", "method": "db_getString", "params": [null, null], "id": "getblock.io"}'
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
