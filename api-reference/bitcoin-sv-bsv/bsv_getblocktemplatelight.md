---
description: >-
  Example code for the getblocktemplatelight  {disallowed} json-rpc method.
  Сomplete guide on how to use getblocktemplatelight  {disallowed} json-rpc in
  GetBlock.io Web3 documentation.
---

# getblocktemplatelight {disallowed} - Bitcoin SV

#### Parameters

`template_request` - json object, optional

A json object in the following spec

`additional_txs` - json array, optional, default=\[]

Hex encoded transactions to add to the block (each tx must be unique and valid)

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getblocktemplatelight",
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
