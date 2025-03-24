---
description: >-
  Example code for the debug_accountRange  {disallowed} json-rpc method.
  Сomplete guide on how to use debug_accountRange  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# debug\_accountRange {disallowed} - Ethereum Classic

#### Parameters

`blockHashOrNumber` - data

Block hash or number.

`txIndex` - integer

Transaction index from which to start.

`address` - data

Address hash from which to start.

`limit` - integer

Maximum number of account entries to return.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_accountRange",
"params": [null, null, null, null],
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
