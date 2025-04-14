---
description: >-
  Example code for the avax.issueTx  {disallowed} json-rpc method. Сomplete
  guide on how to use avax.issueTx  {disallowed} json-rpc in GetBlock.io Web3
  documentation.
---

# avax.issueTx {disallowed} - Avalanche

#### Parameters

`tx` - string

transaction

`encoding` - string

Optional.

encoding specifies the format of the signed transaction. Can be either "cb58" or "hex". Defaults to "cb58".

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/ext/bc/C/rpc' \
--header 'Content-Type: application/json' 
--data-raw '{
  "jsonrpc": "2.0",
  "method": "avax.issueTx",
  "params": [null, null],
  "id": "getblock.io"
}'
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
