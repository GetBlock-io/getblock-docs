---
description: >-
  Example code for the uninstallFilter  {disallowed} json-rpc method. Сomplete
  guide on how to use uninstallFilter  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# uninstallFilter {disallowed} - Binance Smart Chain

#### Parameters

`QUANTITY` - None

The filter id

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "uninstallFilter",
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
