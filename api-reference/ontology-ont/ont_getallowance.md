---
description: >-
  Example code for the getallowance  {disallowed} json-rpc method. Сomplete
  guide on how to use getallowance  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# getallowance {disallowed} - Ontology

#### Parameters

`ont` - string

ammount of allowance

`from` - string

from address

`to` - string

to address

#### Request

```java
curl --location --request POST 'https://ont.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getallowance",
"params": [null, null, null],
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
