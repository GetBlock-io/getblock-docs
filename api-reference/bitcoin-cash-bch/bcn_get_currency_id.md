---
description: >-
  Example code for the get_currency_id  {disallowed} json-rpc method. Сomplete
  guide on how to use get_currency_id  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# get\_currency\_id {disallowed} - Bitcoin Cash

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "get_currency_id",
"params": {},
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
