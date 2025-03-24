---
description: >-
  Example code for the trace_block json-rpc method. Сomplete guide on how to use
  trace_block json-rpc in GetBlock.io Web3 documentation.
---

# trace\_block - Optimism

#### Parameters

`quantity|tag` - string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "trace_block",
"params": ["latest"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32601,
        "message": "the method trace_block does not exist/is not available"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
