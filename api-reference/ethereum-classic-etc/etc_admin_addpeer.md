---
description: >-
  Example code for the admin_addPeer  {disallowed} json-rpc method. Сomplete
  guide on how to use admin_addPeer  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# admin\_addPeer {disallowed} - Ethereum Classic

#### Parameters

`string` - None

Enode URL of peer to add

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "admin_addPeer",
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
