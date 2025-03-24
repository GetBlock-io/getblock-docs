---
description: >-
  Example code for the eth_getMinerDataByBlockNumber json-rpc method. Сomplete
  guide on how to use eth_getMinerDataByBlockNumber json-rpc in GetBlock.io Web3
  documentation.
---

# eth\_getMinerDataByBlockNumber - Ethereum Classic

#### Parameters

`quantity|tag` - string

Integer representing a block number or one of the string tags latest, earliest, or pending, as described in Block Parameter.

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{"jsonrpc": "2.0",
"method": "eth_getMinerDataByBlockNumber",
"params": ["0xcdf167"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "coinbase": "0xb68ed353ea1421668483c46dc9b872aa09ef582a",
        "difficulty": "0x00000000000000000000000000000000000000000000000000011a85f2d724c0",
        "extraData": "0x657a696c2e6d65",
        "netBlockReward": "0x0000000000000000000000000000000000000000000000004575e8cd73bed400",
        "staticBlockReward": "0x0000000000000000000000000000000000000000000000004563918244f40000",
        "totalDifficulty": "0x00000000000000000000000000000000000000000000004ca722cd36ab8b5d6c",
        "transactionFee": "0x0000000000000000000000000000000000000000000000000012574b2ecad400",
        "uncleInclusionReward": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "uncleRewards": []
    }
}
```
