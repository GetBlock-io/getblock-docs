---
description: >-
  Example code for the getblockhash json-rpc method. Сomplete guide on how to
  use getblockhash json-rpc in GetBlock.io Web3 documentation.
---

# getblockhash - Ontology

#### Parameters

`height` - integer

block height

`verbose` - integer

optional

1 for verbose response

#### Request

```java
curl --location --request POST 'https://ont.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockhash",
"params": [16224151, 1],
"id": "getblock.io"}'
```

#### Response

```java
{
    "desc": "SUCCESS",
    "error": 0,
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": "b4553dd4a5b3255aa78d20b2f2f0ec9d3a52d6ef2c99bfa23fd1f67001d9dd8b"
}
```
