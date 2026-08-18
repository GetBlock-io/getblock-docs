---
description: >-
  Example code for the decodepsbt json-rpc method. Сomplete guide on how to use
  decodepsbt json-rpc in GetBlock.io Web3 documentation.
---

# decodepsbt - Bitcoin SV

#### Parameters

`psbt` - string, required

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "decodepsbt",
"params": ["psbt"],
"id": "getblock.io"}'
```

#### Response

```java
null
```
