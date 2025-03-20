---
description: >-
  Example code for the trace_replayBlockTransactions json-rpc method. Сomplete
  guide on how to use trace_replayBlockTransactions json-rpc in GetBlock.io Web3
  documentation.
---

# trace\_replayBlockTransactions - Binance Smart Chain

#### Parameters

`quantity|tag` - None

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`array of strings` - None

Tracing options are trace, vmTrace, and stateDiff. Specify any combination of the three options including none of them.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/v1/mainnet/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "trace_replayBlockTransactions",
"params": ["0x12", ["trace", "vmTrace", "vmTrace"]],
"id": "getblock.io"}'
```

#### Response

```java
{
    "error": {
        "code": -32601,
        "message": "the method trace_replayBlockTransactions does not exist/is not available"
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
