---
description: >-
  Example code for the getnetworkid json-rpc method. Сomplete guide on how to
  use getnetworkid json-rpc in GetBlock.io Web3 documentation.
---

# getnetworkid - Ontology

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getnetworkid",
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
    "result": 1
}
```
