---
description: >-
  Example code for the finalizeblock  {disallowed} json-rpc method. Сomplete
  guide on how to use finalizeblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# finalizeblock {disallowed} - Bitcoin Cash

#### Parameters

`blockhash` - string, required

the hash of the block

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "finalizeblock",
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
