---
description: >-
  Example code for the reconsiderblock  {disallowed} json-rpc method. Сomplete
  guide on how to use reconsiderblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# reconsiderblock {disallowed} - Bitcoin Cash

#### Parameters

`blockhash` - string, required

the hash of the block to reconsider

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "reconsiderblock",
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
