---
description: >-
  Example code for the abandontransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use abandontransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# abandontransaction {disallowed} - Bitcoin Gold

#### Parameters

`txid` - string, required

The transaction id

#### Request

```java
curl --location --request POST 'https://btg.getblock.io/mainnet' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "abandontransaction",
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
