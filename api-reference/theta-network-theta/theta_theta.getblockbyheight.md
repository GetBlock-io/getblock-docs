---
description: >-
  Example code for the theta.GetBlockByHeight json-rpc method. Сomplete guide on
  how to use theta.GetBlockByHeight json-rpc in GetBlock.io Web3 documentation.
---

# theta.GetBlockByHeight - Theta Network

#### Parameters

`height` - string

the block height

#### Request

```java
curl --location --request POST 'https://theta.getblock.io/mainnet/' 
--header 'x-api-key: YOUR-API-KEY' 
--header 'Content-Type: application/json' 
--data-raw '{"jsonrpc": "2.0",
"method": "theta.GetBlockByHeight",
"params": {"height": "11885134"},
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "chain_id": "mainnet",
        "children": [
            "0x33ce33307e0f2d838797a6b075f8d9567fbf5e3ed4043c409aa5269e806dc545"
        ],
        "elite_edge_node_votes": null,
        "epoch": "11967014",
        "guardian_votes": null,
        "hash": "0x571e11a52a269f2f3f6eae033a8889c66f77a9855fb52bc5a89d05b13f8fd77c",
        "hcc": {
            "BlockHash": "0x956bc884ce9ea6f9415fdc4fcbc2f7406ca70ee147dc7f82ec199a5fbcd4e3d1",
            "Votes": [
                {
                    "Block": "0x956bc884ce9ea6f9415fdc4fcbc2f7406ca70ee147dc7f82ec199a5fbcd4e3d1",
                    "Epoch": 11967013,
                    "Height": 11885133,
                    "ID": "0x050bb1210802cf5c624a4b3f501f1c12f68dcc05",
                    "Signature": "0x9c6bda1502d5031b4a17da2a266175e7f1ccea87598a1f85c982ecc2775bb5006593a2106632d2fe68ecba68d392c883dbb9924ff01a63811f030695e9242ff600"
                },
                {
                    "Block": "0x956bc884ce9ea6f9415fdc4fcbc2f7406ca70ee147dc7f82ec199a5fbcd4e3d1",
                    "Epoch": 11967013,
                    "Height": 11885133,
                    "ID": "0x15cc4c3f21417c392119054c8fe5895146e1a493",
                    "Signature": "0x993fcffcdc083c745f619f69918592f45865c4ec38a5b82e2c25d72731406dca6d04c2ade64b80625ed067b5cb6642dd0129b7fce785d5353a5f2ed44295b0c901"
                },
                {
                    "Block": "0x956bc884ce9ea6f9415fdc4fcbc2f7406ca70ee147dc7f82ec199a5fbcd4e3d1",
                    "Epoch": 11967013,
                    "Height": 11885133,
                    "ID": "0x1786d878cb76a53f5950f41fed7d61617e12dfb5",
                    "Signature": "0x353bed24a7a6976553989a2f4d572c887bb7e95772d6a4ac13e4b2a2b7b5588f4669ac1aa902307d66fd392543f035b87e8a0e89f9346267c969890af310c06001"
                }
            ]
        },
        "height": "11885134",
        "parent": "0x956bc884ce9ea6f9415fdc4fcbc2f7406ca70ee147dc7f82ec199a5fbcd4e3d1",
        "proposer": "0x4b80a68a8469d33449eb101082e5500b932a23ce",
        "state_hash": "0xc2b404e63a1e7209c4caed05bdea60e051601c2b52929d227f42fdaf50941ee0",
        "status": 4,
        "timestamp": "1630940573",
        "transactions": [
            {
                "hash": "0x380a2648e772ed7e0460a4a6711d2f0f9fee842450a7ff7c289655d72b76ef9b",
                "raw": {
                    "block_height": "11885133",
                    "outputs": [],
                    "proposer": {
                        "address": "0x4b80a68a8469d33449eb101082e5500b932a23ce",
                        "coins": {
                            "tfuelwei": "0",
                            "thetawei": "0"
                        },
                        "sequence": "0",
                        "signature": "0x72834f1e57f03ad4b623cc425b073e2bd16ba9ccc309f7ad3d127d03a4decf3243d3b7ea4d6e61221ac050d21593518d71025cf54a2e63f8fc31ee8f77e498e701"
                    }
                },
                "receipt": null,
                "type": 0
            },
            {
                "hash": "0xee81850981ecd81eab6a84b9f275ee4c4ff837b953f5097306247d137ac061d7",
                "raw": {
                    "fee": {
                        "tfuelwei": "300000000000000000",
                        "thetawei": "0"
                    },
                    "inputs": [
                        {
                            "address": "0x26239129266c3f46009287294e0ed63fb198de62",
                            "coins": {
                                "tfuelwei": "900000000000000000",
                                "thetawei": "0"
                            },
                            "sequence": "2384808",
                            "signature": "0x6cb14876eca446ce351cca49ba8b2c6066c78dd20b00cbc6078fbc17f0600c3c190a52e572054e3bfff25bed7e86f30c0361e656797cfd74d6dae23f3fa2857501"
                        }
                    ],
                    "outputs": [
                        {
                            "address": "0xc8b3ec135e52515e3b5934a670de67b195f4c96c",
                            "coins": {
                                "tfuelwei": "600000000000000000",
                                "thetawei": "0"
                            }
                        }
                    ]
                },
                "receipt": null,
                "type": 2
            }
        ],
        "transactions_hash": "0x44198353959450f8f7f6a4b3640ec4e57e9b9fb43426295b90c1b280a642f5d0"
    }
}
```
