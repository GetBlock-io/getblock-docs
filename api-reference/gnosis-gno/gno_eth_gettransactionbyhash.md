---
description: >-
  Example code for the eth_getTransactionByHash json-rpc method. Сomplete guide
  on how to use eth_getTransactionByHash json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getTransactionByHash - Gnosis

#### Parameters

`DATA` - hex string

32-byte transaction hash.

#### Request

```java
curl --location --request POST 'https://gno.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getTransactionByHash",
"params": ["0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "blockHash": "0x2eeb97521badd8c3385676d9015b18674141f8a5eedcb3ebe1a913e92f5a7707",
        "blockNumber": "0x1ac46e6",
        "chainId": "0x64",
        "data": "0x66514c970000000000000000000000005b8d36de471880ee21936f328aab2383a280cb2a0000000000000000000000000000000000000000000000068155a43676e0000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000",
        "from": "0x71e11e8b1c1a2bf01934386c959aa65c4288d11e",
        "gas": "0x1a1650",
        "gasPrice": "0xbecb6240",
        "hash": "0xda2b02dbf73f812168844f2fa00c596ee3a15abda8886998d2dba43c49f39174",
        "input": "0x66514c970000000000000000000000005b8d36de471880ee21936f328aab2383a280cb2a0000000000000000000000000000000000000000000000068155a43676e0000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000",
        "maxFeePerGas": "0xbecb6241",
        "maxPriorityFeePerGas": "0xbecb6239",
        "nonce": "0x4a2",
        "r": "0xa9b3b8997bcf5c6768c3be06bffbb4a355c4a77bf82fe457cc5c949537d86ed",
        "s": "0x69f72079570a7c05d5d0213c24c3353ee16baddd66034caf6ec5bd6a9c67011e",
        "to": "0x80dc050a8c923c0051d438026f1192d53033728c",
        "transactionIndex": "0x4",
        "type": "0x2",
        "v": "0x0",
        "value": "0x0",
        "yParity": "0x0"
    }
}
```
