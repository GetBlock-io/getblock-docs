---
description: >-
  Example code for the chain_getBlock json-rpc method. Сomplete guide on how to
  use chain_getBlock json-rpc in GetBlock.io Web3 documentation.
---

# chain\_getBlock - Polkadot

#### Parameters

`hash` - BlockHash

hash of the block

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "chain_getBlock",
"params": ["0x255bc00927df8d33d561792635cbc6bde480a0a505eef5ff28630ece3fc15b32"],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "block": {
            "extrinsics": [
                "0x280403000b406017cb7b01"
            ],
            "header": {
                "digest": {
                    "logs": [
                        "0x0642414245b50103920000001c5b3410000000008c38422de44e2d4810819bffdc072324d552348581a3ee4d08f5d16782e82b1413e24956b9c637cc18b99964298c662274aa7e68fe1e55372e61a65185eddf015588b5d1c11e93a39ef29db473e91b225390cd155dfdb9bff7d29dd7cf897003",
                        "0x0542414245010114a5c414eb433fef25d514ac35937c1c2b83ffbac8a47479f02393172f622d02789eb0e3c4a8281f9a169a5fcf19cb1c8285f6382fc7ce3d0b71f82ca3f03582"
                    ]
                },
                "extrinsicsRoot": "0xada254ef8321e28a6667ad75659be6464944174bd5540667da94590a0a4a596f",
                "number": "0x67103a",
                "parentHash": "0x570e3f417a41646d9b978bf2ac3d68be48bb0f73082825f438af58a37cfe0ef8",
                "stateRoot": "0x176c67eca385c24403ba774550ff75dcfa652d6d6cf2d2ecbccf56e513db601c"
            }
        },
        "justifications": null
    }
}
```
