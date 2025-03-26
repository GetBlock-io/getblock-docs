---
description: >-
  Example code for the listunspent  {disallowed} json-rpc method. Сomplete guide
  on how to use listunspent  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# listunspent {disallowed} - Bitcoin Cash

#### Parameters

`minconf` - numeric, optional, default=1

The minimum confirmations to filter

`maxconf` - numeric, optional, default=9999999

The maximum confirmations to filter

`addresses` - json array, optional, default=empty array

The bitcoin addresses to filter

`include_unsafe` - boolean, optional, default=true

Include outputs that are not safe to spend. See description of “safe” attribute below.

`query_options` - json object, optional

JSON with query options

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "listunspent",
"params": [null, null, null, null, null],
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
