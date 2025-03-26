---
description: >-
  Example code for the submitblock  {disallowed} json-rpc method. Сomplete guide
  on how to use submitblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# submitblock {disallowed} - Bitcoin

#### Parameters

`hexdata` - string, required

the hex-encoded block data to submit

`dummy` - string, optional, default=ignored

Dummy value, for compatibility with BIP22. This value is ignored.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "submitblock",
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
