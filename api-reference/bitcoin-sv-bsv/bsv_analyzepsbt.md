---
description: >-
  Example code for the analyzepsbt json-rpc method. Сomplete guide on how to use
  analyzepsbt json-rpc in GetBlock.io Web3 documentation.
---

# analyzepsbt - Bitcoin SV

#### Parameters

`psbt` - string, required

A base64 string of a PSBT

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "analyzepsbt",
"params": ["psbt"],
"id": "getblock.io"}'
```
