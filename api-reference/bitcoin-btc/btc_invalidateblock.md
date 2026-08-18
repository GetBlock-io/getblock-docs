---
description: >-
  Example code for the invalidateblock  {disallowed} json-rpc method. Сomplete
  guide on how to use invalidateblock  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# invalidateblock {disallowed} - Bitcoin

#### Parameters

`blockhash` - string, required

the hash of the block to mark as invalid

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "invalidateblock",
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
