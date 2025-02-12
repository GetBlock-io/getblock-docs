---
description: >-
  Example code for the eth_call  {disallowed} json-rpc method. Сomplete guide on
  how to use eth_call  {disallowed} json-rpc in GetBlock.io Web3 documentation.
---

# eth\_call {disallowed} - Moonriver

#### Parameters

`request` - EthCallRequest

None

`number` - BlockNumber

None

#### Request

```java
curl --location --request POST 'https://movr.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_call",
"params": [null, null],
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
