---
description: >-
  Example code for the getconnectioncount json-rpc method. Сomplete guide on how
  to use getconnectioncount json-rpc in GetBlock.io Web3 documentation.
---

# getconnectioncount - Ontology

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getconnectioncount",
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
    "result": 33
}
```
