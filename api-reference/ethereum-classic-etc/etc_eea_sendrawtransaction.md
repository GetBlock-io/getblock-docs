---
description: >-
  Example code for the eea_sendRawTransaction  {disallowed} json-rpc method.
  Сomplete guide on how to use eea_sendRawTransaction  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# eea\_sendRawTransaction {disallowed} - Ethereum Classic

#### Parameters

`data` - None

Signed RLP-encoded private transaction.

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "eea_sendRawTransaction",
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
