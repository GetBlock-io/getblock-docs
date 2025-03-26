---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Binance Smart Chain

#### Parameters

`QUANTITY|TAG` - hex string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

`Boolean` - boolean

If true, returns the full transaction objects; if false, returns only the hashes of the transactions.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByNumber",
"params": ["0x68B3", true],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "difficulty": "0x2",
        "extraData": "0xd883010002846765746888676f312e31332e34856c696e7578000000000000003a296bf3aaac8eb5a96709d4666dd9f3825bec9ea51e3a4ae3bf6666d5e0dece192acc00999b6c9afb361ef8b81f416f54260816a6660dcaaff77f472e812f7a01",
        "gasLimit": "0x1c9c380",
        "gasUsed": "0x0",
        "hash": "0xf962d3908e00ce08da5fba7f49fc58e2739e41fc6e029562aa7197feb8ad01ec",
        "logsBloom": "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
        "miner": "0x685b1ded8013785d6623cc18d214320b6bb64759",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0x68b3",
        "parentHash": "0x033272e654c39553fb220e86c73c0bfbf2e11905b6848cdd0ab348622577b55f",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x261",
        "stateRoot": "0x90b43bf5f1c6f69fdcc6d90900077f76065f7052c02bdc85e701b20328de7a27",
        "timestamp": "0x5f4b05bd",
        "totalDifficulty": "0xd0d8",
        "transactions": [],
        "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "uncles": []
    }
}
```
