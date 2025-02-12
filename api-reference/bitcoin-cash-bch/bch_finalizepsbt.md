---
description: >-
  Example code for the finalizepsbt json-rpc method. Сomplete guide on how to
  use finalizepsbt json-rpc in GetBlock.io Web3 documentation.
---

# finalizepsbt - Bitcoin Cash

#### Parameters

`psbt` - string, required

A base64 string of a PSBT

`extract` - boolean, optional, default=true

If true and the transaction is complete, extract and return the complete transaction in normal network serialization instead of the PSBT.

#### Request

```java
curl --location --request POST 'https://bch.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "finalizepsbt",
"params": ["psbt", null],
"id": "getblock.io"}'
```

#### Response

```java
null
```
