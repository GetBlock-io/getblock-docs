---
description: >-
  Example code for the settxfee  {disallowed} json-rpc method. Сomplete guide on
  how to use settxfee  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# settxfee {disallowed} - Dash

#### Parameters

`Transaction Fee Per Kilobyte` - number (dash)

The transaction fee to pay, in dash, for each kilobyte of transaction data. Be careful setting the fee too low---your transactions may not be relayed or included in blocks.

#### Request

```java
curl --location --request POST 'https://dash.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "settxfee",
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
