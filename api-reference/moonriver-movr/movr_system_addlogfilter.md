---
description: >-
  Example code for the system_addLogFilter  {disallowed} json-rpc method.
  Сomplete guide on how to use system_addLogFilter  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# system\_addLogFilter {disallowed} - Moonriver

#### Parameters

`directives` - Text

None

#### Request

```java
curl --location --request POST 'https://movr.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "system_addLogFilter",
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
