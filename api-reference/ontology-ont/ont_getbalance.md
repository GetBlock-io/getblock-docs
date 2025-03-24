---
description: >-
  Example code for the getbalance json-rpc method. Сomplete guide on how to use
  getbalance json-rpc in GetBlock.io Web3 documentation.
---

# getbalance - Ontology

#### Parameters

`address` - string

Base58 address

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "getbalance",
"params": ["Adj7W5Z2hTeKH7YwJsfMzLuwiD671mvJ6X"],
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
        "height": "16224577",
        "ong": "9380961617034",
        "ont": "14540"
    }
}
```
