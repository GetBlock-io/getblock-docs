---
description: >-
  Example code for the importdescriptors  {disallowed} json-rpc method. Сomplete
  guide on how to use importdescriptors  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# importdescriptors {disallowed} - Bitcoin Gold

#### Parameters

`requests` - json array, required

Data to be imported

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "importdescriptors",
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
