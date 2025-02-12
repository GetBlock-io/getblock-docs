---
description: >-
  Example code for the admin_logsRemoveCache  {disallowed} json-rpc method.
  Сomplete guide on how to use admin_logsRemoveCache  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# admin\_logsRemoveCache {disallowed} - Ethereum Classic

#### Parameters

`fromBlock` - None

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`toBlock` - None

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "admin_logsRemoveCache",
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
