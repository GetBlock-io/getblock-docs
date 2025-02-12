---
description: >-
  Example code for the combinepsbt json-rpc method. Сomplete guide on how to use
  combinepsbt json-rpc in GetBlock.io Web3 documentation.
---

# combinepsbt - Bitcoin SV

#### Parameters

`txs` - json array, required

A json array of base64 strings of partially signed transactions

#### Request

```java
curl --location --request POST 'https://bsv.getblock.io/mainnet/' \ 
--header 'x-api-key: YOUR-API-KEY' \ 
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "combinepsbt",
"params": [["mybase64_1", "mybase64_2", "mybase64_3"]],
"id": "getblock.io"}'
```

#### Response

```java
null
```
