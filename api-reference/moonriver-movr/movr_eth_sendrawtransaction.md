---
description: >-
  Example code for the eth_sendRawTransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use eth_sendRawTransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_sendRawTransaction {disallowed} - Moonriver

#### Parameters

`bytes` - Bytes

None

#### Request

```java
curl --location --request POST 'https://movr.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_sendRawTransaction",
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
