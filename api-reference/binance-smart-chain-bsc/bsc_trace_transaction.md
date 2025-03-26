---
description: >-
  Example code for the trace_transaction json-rpc method. Сomplete guide on how
  to use trace_transaction json-rpc in GetBlock.io Web3 documentation.
---

# trace\_transaction - Binance Smart Chain

#### Parameters

`data` - None

Transaction hash

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "trace_transaction",
"params": ["0x4c253746668dca6ac3f7b9bc18248b558a95b5fc881d140872c2dff984d344a7"],
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
