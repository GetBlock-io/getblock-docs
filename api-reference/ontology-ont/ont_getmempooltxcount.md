---
description: >-
  Example code for the getmempooltxcount json-rpc method. Сomplete guide on how
  to use getmempooltxcount json-rpc in GetBlock.io Web3 documentation.
---

# getmempooltxcount - Ontology

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getmempooltxcount",
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
    "result": [
        1,
        0
    ]
}
```
