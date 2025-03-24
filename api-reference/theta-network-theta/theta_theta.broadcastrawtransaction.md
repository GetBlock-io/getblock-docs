---
description: >-
  Example code for the theta.BroadcastRawTransaction json-rpc method. Сomplete
  guide on how to use theta.BroadcastRawTransaction json-rpc in GetBlock.io Web3
  documentation.
---

# theta.BroadcastRawTransaction - Theta Network

#### Parameters

`tx_bytes` - string

the signed transaction bytes.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "theta.BroadcastRawTransaction",
"params": {"tx_bytes": "02f8a4c78085e8d4a51000f86ff86d942e833968e5bb786ae419c4d13189fb081cc43babd3888ac7230489e800008901158e46f1e875100015b841c2daae6cab92e37308763664fcbe93d90219df5a3520853a9713e70e734b11f27a43db6b77da4f885213b45a294c2b4c74dc9a018d35ba93e5b9297876a293c700eae9949f1233798e905e173560071255140b4a8abd3ec6d3888ac7230489e800008901158e460913d00000"},
"id": "getblock.io"}'
```

#### Response

```java
{
    "result": {
        "hash": "0x0a495698654ff5372ef8936eca727c25b975ea0f7e5ebea282e3c86017dfe521",
        "block": {
            "ChainID": "privatenet",
            "Epoch": 40186,
            "Height": 20094,
            "Parent": "0x6797d42ff724a47e9dfd3aa425e2a4f06f40b64fd347ca8c9ebb43d08cb58847",
            "HCC": {
                "Votes": {},
                "BlockHash": "0x6797d42ff724a47e9dfd3aa425e2a4f06f40b64fd347ca8c9ebb43d08cb58847"
            },
            "TxHash": "0xbed5492c96d8251c2f0846a2d12d7a17eb88966a736fa0e8f96af36ed3ffaa74",
            "ReceiptHash": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
            "Bloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "StateHash": "0xd2b1fa73461fe80d266933033df073a2eb5b82d16cad46f2f74fa2b575adbc88",
            "Timestamp": 1548272003,
            "Proposer": "0x2e833968e5bb786ae419c4d13189fb081cc43bab",
            "Signature": "0x514b731086640c79122f81db1ac1a142d39a92393affb3cbaa09df395dbb441a4b735a66d0da1a52887b66a155033e51b36584dd14b68616fe30b0f2a3f434fa01"
        }
    },
    "id": "getblock.io",
    "jsonrpc": "2.0"
}
```
