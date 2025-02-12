---
description: >-
  Example code for the debug_traceCall json-rpc method. Сomplete guide on how to
  use debug_traceCall json-rpc in GetBlock.io Web3 documentation.
---

# debug\_traceCall - Binance Smart Chain

#### Parameters

`Object` - None

The transaction call object.

`DATA` - None

The block number in hex format, tags or the block hash.

`DATA` - None

The type of tracer.

#### Request

```java
curl --location --request POST 'https://bsc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "debug_traceCall",
"params": [{"to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567"}, "finalized", {"tracer": "callTracer"}],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "from": "0x0000000000000000000000000000000000000000",
        "gas": "0x17d2638",
        "gasUsed": "0x5208",
        "input": "0x",
        "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
        "type": "CALL",
        "value": "0x0"
    }
}
```
