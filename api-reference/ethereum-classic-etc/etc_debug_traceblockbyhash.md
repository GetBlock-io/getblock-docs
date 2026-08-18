---
description: >-
  Example code for the debug_traceBlockByHash  {disallowed} json-rpc method.
  Сomplete guide on how to use debug_traceBlockByHash  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# debug\_traceBlockByHash {disallowed} - Ethereum Classic

#### Parameters

`block hash` - data

Block hash.

`Object` - None

request options (all optional and default to false)

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "debug_traceBlockByHash",
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
