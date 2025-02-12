---
description: >-
  Example code for the db_putString  {disallowed} json-rpc method. Сomplete
  guide on how to use db_putString  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# db\_putString {disallowed} - Fuse Network

#### Parameters

`database` - string

database name

`key` - string

key name

`string` - string

string to store.

#### Request

```java
curl --location --request POST 'https://fuse.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "db_putString",
"params": [null, null, null],
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
