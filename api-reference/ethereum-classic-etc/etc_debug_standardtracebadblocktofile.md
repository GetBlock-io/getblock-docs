---
description: >-
  Example code for the debug_standardTraceBadBlockToFile  {disallowed} json-rpc
  method. Сomplete guide on how to use debug_standardTraceBadBlockToFile 
  {disallowed} json-rpc in GetBlock.io Web3 document
---

# debug\_standardTraceBadBlockToFile {disallowed} - Ethereum Classic

#### Parameters

`blockHash` - data

Block hash

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_standardTraceBadBlockToFile",
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
