---
description: >-
  Example code for the rescanblockchain  {disallowed} json-rpc method. Сomplete
  guide on how to use rescanblockchain  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# rescanblockchain {disallowed} - Bitcoin Cash

#### Parameters

`start_height` - numeric, optional, default=0

block height where the rescan should start

`stop_height` - numeric, optional

the last block height that should be scanned. If none is provided it will rescan up to the tip at return time of this call.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "rescanblockchain",
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
