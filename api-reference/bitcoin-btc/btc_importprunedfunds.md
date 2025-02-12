---
description: >-
  Example code for the importprunedfunds  {disallowed} json-rpc method. Сomplete
  guide on how to use importprunedfunds  {disallowed} json-rpc in GetBlock.io
  Web3 documentation.
---

# importprunedfunds {disallowed} - Bitcoin

#### Parameters

`rawtransaction` - string, required

A raw transaction in hex funding an already-existing address in wallet

`txoutproof` - string, required

The hex output from gettxoutproof that contains the transaction

#### Request

```java
curl --location --request POST 'https://btc.getblock.io/mainnet/' \
--header 'x-api-key: YOUR-API-KEY' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "importprunedfunds",
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
