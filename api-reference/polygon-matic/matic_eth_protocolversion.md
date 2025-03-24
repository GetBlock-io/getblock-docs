---
description: >-
  Example code for the eth_protocolVersion json-rpc method. Сomplete guide on
  how to use eth_protocolVersion json-rpc in GetBlock.io Web3 documentation.
---

# eth\_protocolVersion - Polygon

#### Parameters

\-

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_protocolVersion",
"params": [],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32601,
        "message": "the method eth_protocolVersion does not exist/is not available"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
