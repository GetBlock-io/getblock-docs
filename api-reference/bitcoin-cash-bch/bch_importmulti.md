---
description: >-
  Example code for the importmulti  {disallowed} json-rpc method. Сomplete guide
  on how to use importmulti  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# importmulti {disallowed} - Bitcoin Cash

#### Parameters

`requests` - json array, required

Data to be imported

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "importmulti",
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
