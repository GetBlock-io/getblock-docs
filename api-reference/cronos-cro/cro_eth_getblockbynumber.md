---
description: >-
  Example code for the eth_getBlockByNumber json-rpc method. Сomplete guide on
  how to use eth_getBlockByNumber json-rpc in GetBlock.io Web3 documentation.
---

# eth\_getBlockByNumber - Cronos

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
"params": ["0x8252EC", true],
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
            {
                "accessList": [],
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "chainId": "0x19",
                "from": "0x8a4f3a4ed9af1a85d9e455dd8d1222507f9f3702",
                "gas": "0x6c22f",
                "gasPrice": "0x44fccac8840",
                "hash": "0xc7c1d08c38bb7f0606d4e4f526f38bdd62dc1cb42a677bcb9ffea76f2f15ad59",
                "input": "0xe2bbb1580000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000008619b51cb1b96f5cf",
                "maxFeePerGas": "0x5d201d51bf7",
                "maxPriorityFeePerGas": "0x59682f00",
                "nonce": "0x2fb",
                "r": "0x9f7fa9538baab5006bbe050b1f7fafb34c2ee9d032e520968e71e16515613633",
                "s": "0x647cf298a25d6d4a4a285565b5f82cc3e462746c3229ca964597f15e87206af7",
                "to": "0xab50fb1117778f293cc33ac044b5579fb03029d0",
                "transactionIndex": "0x0",
                "type": "0x2",
                "v": "0x0",
                "value": "0x0"
            },
            {
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "from": "0x223b1e0d9e15f6bff17ff814fced2724eb78d8be",
                "gas": "0x30d40",
                "gasPrice": "0x44f73473675",
                "hash": "0xb27e60afdd656fb7a3db9122e66b3d8468e7529caea9bfc79b5cf3e5cb588777",
                "input": "0xa9059cbb000000000000000000000000ec912e4f6baceaa63e30e3f010508171cf5aa5240000000000000000000000000000000000000000023152f2c00e8884498e13f9",
                "nonce": "0x17a",
                "r": "0x6368281cd95f849d2bb97fda3def460b9dc88296b48cc5293ca343a1bb60a2b1",
                "s": "0x138113e641999e1be42ab1b246c07126253b9c64792a1eb9b5d69ae244df3ced",
                "to": "0xdd73dea10abc2bff99c60882ec5b2b81bb1dc5b2",
                "transactionIndex": "0x1",
                "type": "0x0",
                "v": "0x55",
                "value": "0x0"
            },
            {
                "blockHash": "0x08840bf78ffc1aebc8237d3c3d209f8337f0e0c2f975e5b1c3eac816d28d760e",
                "blockNumber": "0x8252ec",
                "from": "0x227f6757289a86c13eee2e91c2e6eb03f2ed11a6",
                "gas": "0xf4240",
                "gasPrice": "0x44f7347ed65",
                "hash": "0x05595f42e607777285b2e2e93f69ea5b20e9d3269e0b6f4caecd17d12eb9c768",
                "input": "0x38ed173900000000000000000000000000000000000000000367d92e7d6fba5cde0000000000000000000000000000000000000000000000000000000000000009fd101c00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000227f6757289a86c13eee2e91c2e6eb03f2ed11a6000000000000000000000000000000000000000000000000000000006473d2180000000000000000000000000000000000000000000000000000000000000003000000000000000000000000dd73dea10abc2bff99c60882ec5b2b81bb1dc5b20000000000000000000000002d03bece6747adc00e1a131bba1469c15fd11e03000000000000000000000000c21223249ca28397b4b6541dffaecc539bff0c59",
                "nonce": "0x30750",
                "r": "0x4fdf66fca692d396412e62f3e7664211b0722117ce30731c5628c2fa9a22ad0e",
                "s": "0x5723052482c4d3b4bfcb74742e4f24f04fa26f7480d1a432f17b47531bb1fa8e",
                "to": "0x145863eb42cf62847a6ca784e6416c1682b1b2ae",
                "transactionIndex": "0x2",
                "type": "0x0",
                "v": "0x56",
                "value": "0x0"
            }
        ],
        "transactionsRoot": "0xa802cdd8cea1a6058eda002d311516497364d31d98994bded9c87932bd62bd29",
        "uncles": []
    }
}
```
