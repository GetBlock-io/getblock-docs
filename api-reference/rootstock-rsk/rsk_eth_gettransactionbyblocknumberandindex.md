---
description: >-
  Example code for the eth_getTransactionByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getTransactionByBlockNumberAndIndex json-rpc
  in GetBlock.io Web3 documentation.
---

# eth\_getTransactionByBlockNumberAndIndex - Rootstock

#### Parameters

`QUANTITY|TAG` - integer or string

block number or "latest", "earliest" or "pending"

`QUANTITY` - integer

Transaction index position.

#### Request

```java
wscat -c wss://go.getblock.io/<ACCESS-TOKEN>/ 
# wait for connection and send the request body 
{"jsonrpc": "2.0",
"method": "eth_getTransactionByBlockNumberAndIndex",
"params": ["latest", "0x0"],
"id": "getblock.io"}
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0xce6eaa95170f1d59d5bd971a4a8e3de38e795a063fb1ebcfa3e9d5a6af1635a3",
        "blockNumber": "0x526f5f",
        "from": "0xd554a563adfd9198782add9ad7554c6ffe87ef09",
        "gas": "0x30d40",
        "gasPrice": "0x3ec6f15",
        "hash": "0xbbf9233fc0040143409d9cfdc81243359f4c7488d71aa678d6f262f11be4c6aa",
        "input": "0x5a6866990000000000000000000000000000000000000000000000000134227e9baf1c4900000000000000000000000000000000000000000000000000000000649078f8000000000000000000000000504efcadfb020d6bbaec8a5c5bb21453719d0e00",
        "nonce": "0x79d8a",
        "r": "0xb158144529857b29cb15c51afbac2205d7952f31a10765b10d4784eb6490f022",
        "s": "0x5c35d69ecb29ef2223ee2b986f64b6a7482e7cc59ec89689788c34dcd14f2fa0",
        "to": "0x461750b4824b14c3d9b7702bc6fbb82469082b23",
        "transactionIndex": "0x0",
        "type": "0x0",
        "v": "0x60",
        "value": "0x0"
    }
}
```
