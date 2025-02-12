---
description: >-
  Example code for the hmy_getBlockByNumber json-rpc method. Сomplete guide on
  how to use hmy_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# hmy\_getBlockByNumber - Harmony

#### Parameters

`QUANTITY|TAG` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`Boolean` - boolean

If true, returns the full transaction objects; if false, returns only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://one.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "hmy_getBlockByNumber",
"params": ["0x8252EC", true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "difficulty": 0,
        "epoch": "0x1b1",
        "extraData": "0x",
        "gasLimit": "0x4c4b400",
        "gasUsed": "0x0",
        "hash": "0x150b67519eeb0694d5651f3f51a7448f3e7de3843a80281c87d48acb6c4b53b8",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "miner": "one1gh043zc95e6mtutwy5a2zhvsxv7lnlklkj42ux",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": 0,
        "number": "0x8252ec",
        "parentHash": "0x1148b6d29fe80cc536cc4b8271e1c6b64e321d9c32124b3cd4ee2aab2ffd5660",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "size": "0x590",
        "stakingTransactions": [],
        "stateRoot": "0x215566cfecbd28f6f58331fe0cc113617da73804fc4f69bab0ea6bd7dea55ab1",
        "timestamp": "0x6013f7e7",
        "transactions": [],
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "uncles": [],
        "viewID": "0x825418",
        "vrf": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "vrfProof": "0x"
    }
}
```
