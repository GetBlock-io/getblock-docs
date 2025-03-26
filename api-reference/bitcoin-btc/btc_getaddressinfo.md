---
description: >-
  Example code for the getaddressinfo  {disallowed} json-rpc method. Сomplete
  guide on how to use getaddressinfo  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getaddressinfo {disallowed} - Bitcoin

#### Parameters

`address` - string, required

The bitcoin address for which to get information.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getaddressinfo",
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
