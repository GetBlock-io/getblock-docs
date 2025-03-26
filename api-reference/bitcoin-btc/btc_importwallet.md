---
description: >-
  Example code for the importwallet  {disallowed} json-rpc method. Сomplete
  guide on how to use importwallet  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# importwallet {disallowed} - Bitcoin

#### Parameters

`filename` - string, required

The wallet file

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "importwallet",
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
