---
description: >-
  Example code for the eth_getUncleByBlockNumberAndIndex json-rpc method.
  Сomplete guide on how to use eth_getUncleByBlockNumberAndIndex json-rpc in
  GetBlock.io Web3 documentation.
---

# eth\_getUncleByBlockNumberAndIndex - Ethereum Classic

#### Parameters

`quantity|tag` - string

Index of the block, or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`quantity` - None

Index of the uncle.

#### Request

```java
curl --location --request POST 'https://etc.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getUncleByBlockNumberAndIndex",
"params": ["latest", "0x0"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "difficulty": "0x77daec467bf93",
        "extraData": "0x50505945206e616e6f706f6f6c2e6f7267",
        "gasLimit": "0x7a121d",
        "gasUsed": "0x7a0f7b",
        "hash": "0x42d83ae9c0743f4b1f9c61ff7ea8b164c1bab3627decd49233760680be006ecf",
        "logsBloom": "0x888200800000340120220008640200500408006100038400100581c000080240080a0014e8002010080004088040004022402a000c18010001400100002a041141a0610a0052900600041018c0002a0003090020404c00206010010513d00020005380124e08050480710000000108401012b0901c1424006000083a10a8c1040100a0440081050210124400040044304070004001100000012600806008061d0320800000b40042160600002480000000800000c0002100200940801c000820800048024904710000400640490026000a44300309000286088010c2300060003011380006400200812009144042204810209020410a84000410520c08802941",
        "miner": "0x52bc44d5378309ee2abf1539bf71de1b7d7be3b5",
        "mixHash": "0xf977fcdb52868be410b75ef2becc35cc312f13ab0a6ce400ecd9d445f66fa3f2",
        "nonce": "0x628b28403bf1e3d3",
        "number": "0x7689d0",
        "parentHash": "0xb32cfdfbf4adb05d30f02fcc6fe039cc6666402142954051c1a1cb9cc91aa11e",
        "receiptsRoot": "0x9c7c8361d1a24ea2841432234c81974a9920d3eba2b2b1c496b5f925a95cb4ac",
        "sha3Uncles": "0x7d972aa1b182b7e93f1db043f03fbdbfac6874fe7e67e162141bcc0aefa6336b",
        "size": "0x21a",
        "stateRoot": "0x74e97b77813146344d75acb5a52a006cc6dfaca678a10fb8a484a8443e919272",
        "timestamp": "0x5cdcc0a7",
        "totalDifficulty": "0x229b0583b4bd2698ca0",
        "transactionsRoot": "0x1d21626afddf05e5866de66ca3fcd98f1caf5357eba0cc6ec675606e116a891b",
        "uncles": []
    }
}
```
