---
description: >-
  Example code for the getgasprice json-rpc method. Сomplete guide on how to use
  getgasprice json-rpc in GetBlock.io Web3 documentation.
---

# getgasprice - Ontology

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getgasprice",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "desc": "SUCCESS",
    "error": 0,
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "gasprice": 2500,
        "height": 16224576
    }
}
```
