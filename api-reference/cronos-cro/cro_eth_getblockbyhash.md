---
description: >-
  Example code for the eth_getBlockByHash json-rpc method. Сomplete guide on how
  to use eth_getBlockByHash json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByHash - Cronos

#### Parameters

`DATA` - hex string

32-byte hash of a block.

`Boolean` - boolean

If true, returns the full transaction objects; if false, returns the transaction hashes.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getBlockByHash",
"params": ["0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e", false],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "baseFeePerGas": "0x44f73445940",
        "difficulty": "0x0",
        "extraData": "0x",
        "gasLimit": "0x2625a00",
        "gasUsed": "0xf8973",
        "hash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
        "logsBloom": "0x0020080000000000080004008000000000020000040800000000000000200000200000000000000000000000410000020800000003000400000440004020428000200000008000800800000800020020000004000000000040000100000000000000040002000000000000000081080000000100000110000000001000000400000000004000c00080000000010000000000000000000008100000c000000000020000000000200084600000040000000020080040880000001100000000400000000002000000001002040008000800000000000000001000000000001020000010010000000000000000002000000000000001400000000022000000000000",
        "miner": "0xca5cf03d081197be24ef707081fbd7f3f11eb02d",
        "mixHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "nonce": "0x0000000000000000",
        "number": "0x8252ec",
        "parentHash": "0x534705440591fb5ac1b744020f3afc39dfa7512fe32a5ab851e9678f19974658",
        "receiptsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
        "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
        "size": "0x1336",
        "stateRoot": "0x5601c19dd5b59450232b65f1c3a82f0efd7ffeab6c83d4461668609e0feaa720",
        "timestamp": "0x6473d0f8",
        "totalDifficulty": "0x0",
        "transactions": [
            "0xc7c1d08c38bb7f0606d4e4f526f38bdd62dc1cb42a677bcb9ffea76f2f15ad59",
            "0xb27e60afdd656fb7a3db9122e66b3d8468e7529caea9bfc79b5cf3e5cb588777",
            "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768"
        ],
        "transactionsRoot": "0xa802cdd8cea1a6058eda002d311516497364d31d98994bded9c87932bd62bd29",
        "uncles": []
    }
}
```
