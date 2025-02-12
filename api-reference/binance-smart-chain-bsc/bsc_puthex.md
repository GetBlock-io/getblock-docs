---
description: >-
  Example code for the putHex  {disallowed} json-rpc method. Сomplete guide on
  how to use putHex  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# putHex {disallowed} - Binance Smart Chain

#### Parameters

`String` - None

Database name

`String` - None

Key name

`DATA` - None

The data to store

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "putHex",
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
