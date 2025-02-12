---
description: >-
  Example code for the utxoupdatepsbt json-rpc method. Сomplete guide on how to
  use utxoupdatepsbt json-rpc in GetBlock.io Web3 documentation.
---

# utxoupdatepsbt - Bitcoin SV

#### Parameters

`psbt` - string, required

A base64 string of a PSBT

`descriptors` - json array, optional

An array of either strings or objects

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "utxoupdatepsbt",
"params": ["psbt", null],
"id": "getblock.io"}'
```
