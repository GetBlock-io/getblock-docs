---
description: >-
  Example code for the trace_transaction json-rpc method. Сomplete guide on how
  to use trace_transaction json-rpc in GetBlock.io Web3 documentation.
---

# trace\_transaction - Optimism

#### Parameters

`data` - string

Transaction hash

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "trace_transaction",
"params": ["0x01736b023300e297e3b3c7c13ab5db49c7c0be768d48790f46dd7baeefa7b047"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32601,
        "message": "the method trace_transaction does not exist/is not available"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
