---
description: >-
  Example code for the getreceivedbyaddress  {disallowed} json-rpc method.
  Сomplete guide on how to use getreceivedbyaddress  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getreceivedbyaddress {disallowed} - Bitcoin

#### Parameters

`address` - string, required

The bitcoin address for transactions.

`minconf` - numeric, optional, default=1

Only include transactions confirmed at least this many times.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getreceivedbyaddress",
"params": [null, null],
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
