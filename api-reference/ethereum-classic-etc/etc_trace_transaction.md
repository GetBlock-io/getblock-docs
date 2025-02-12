---
description: >-
  Example code for the trace_transaction json-rpc method. Сomplete guide on how
  to use trace_transaction json-rpc in GetBlock.io Web3 documentation.
---

# trace\_transaction - Ethereum Classic

#### Parameters

`data` - string

Transaction hash

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "trace_transaction",
"params": ["0x01736b023300e297e3b3c7c13ab5db49c7c0be768d48790f46dd7baeefa7b047"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": [
        {
            "action": {
                "callType": "call",
                "from": "0x7eb4c9d6b763324eea4852f5d40985bbf0f29832",
                "gas": "0x40054",
                "input": "0xfe97ea430000000000000000000000007eb4c9d6b763324eea4852f5d40985bbf0f298320000000000000000000000000000000000000000000000000000000000000017000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000000160dfb255ad11a4d8d2a4a6e264b0b41baf04737047bde86bfe5e16f27244215400000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000",
                "to": "0x3c42649799074b438889b80312ea9f62bc798aa8",
                "value": "0x0"
            },
            "blockHash": "0xbb50cb8d2d5698c1b10f1a845360ecf521ff18b08f71664a4639b6bdbb08fb38",
            "blockNumber": 13496679,
            "result": {
                "gasUsed": "0x15b60",
                "output": "0x"
            },
            "subtraces": 0,
            "traceAddress": [],
            "transactionHash": "0x01736b023300e297e3b3c7c13ab5db49c7c0be768d48790f46dd7baeefa7b047",
            "transactionPosition": 0,
            "type": "call"
        }
    ]
}
```
