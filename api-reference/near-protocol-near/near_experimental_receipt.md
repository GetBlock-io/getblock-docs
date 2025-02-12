---
description: >-
  Example code for the EXPERIMENTAL_receipt  {disallowed} json-rpc method.
  Сomplete guide on how to use EXPERIMENTAL_receipt  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# EXPERIMENTAL\_receipt {disallowed} - NEAR Protocol

#### Parameters

`id` - string

receipt id

#### Request

```java
curl --location --request POST 'https://near.getblock.io/mainnet' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "EXPERIMENTAL_receipt",
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
