---
description: >-
  Example code for the getblockheightbytxhash json-rpc method. Сomplete guide on
  how to use getblockheightbytxhash json-rpc in GetBlock.io Web3 documentation.
---

# getblockheightbytxhash - Ontology

#### Parameters

`hash` - string

transaction hash

#### Request

```java
curl --location --request POST 'https://ont.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "getblockheightbytxhash",
"params": ["db81c4e00f050c4f8e2f9c7e8201fddebd18458b3c48c73c18aa0532c7b5c43c"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "desc": "SUCCESS",
    "error": 0,
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": 16224161
}
```
