---
description: >-
  Example code for the getbestblockhash json-rpc method. Сomplete guide on how
  to use getbestblockhash json-rpc in GetBlock.io Web3 documentation.
---

# getbestblockhash - Ontology

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "getbestblockhash",
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
    "result": "449822980132ae033acf72fd5530db33a3963964d51c7ce81fe3aadb763a50b4"
}
```
