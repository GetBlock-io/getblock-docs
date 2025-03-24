---
description: >-
  Example code for the validators json-rpc method. Сomplete guide on how to use
  validators json-rpc in GetBlock.io Web3 documentation.
---

# validators - NEAR Protocol

#### Parameters

`hash or number` - string or int

Optional

hash or number of the latest block

#### Request

```java
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \ 
--data-raw '{"jsonrpc": "2.0",
"method": "validators",
"params": [null],
"id": "getblock.io"}'
```

#### Response

```java
{
    "id": "getblock.io",
    "jsonrpc": "2.0",
    "result": {
        "current_fishermen": [],
        "current_proposals": [
            {
                "account_id": "01node.poolv1.near",
                "public_key": "ed25519:5xz7EbcnPqabwoFezdJBxieK8S7XLsdHHuLwM4vLLhFt",
                "stake": "2269885917418553047472319475913",
                "validator_stake_struct_version": "V1"
            },
            {
                "account_id": "08investinwomen_runbybisontrails.poolv1.near",
                "public_key": "ed25519:C6yqxQ3suwjmm8ufG5e3BsHiwxUs9h839FCneF41V7TM",
                "stake": "5861363862832431885948505306183",
                "validator_stake_struct_version": "V1"
            },
            {
                "account_id": "4ire.poolv1.near",
                "public_key": "ed25519:GdirfFSuT3t1ChEMqwcV6zko5Ya9yVAqm6Pnhk9qcXvS",
                "stake": "139835367068368402112668541417",
                "validator_stake_struct_version": "V1"
            }
        ],
        "current_validators": [
            {
                "account_id": "staked.poolv1.near",
                "is_slashed": false,
                "num_expected_blocks": 1471,
                "num_expected_chunks": 5884,
                "num_produced_blocks": 1471,
                "num_produced_chunks": 5884,
                "public_key": "ed25519:3JBVXqenru2ErAM1kHQ8qfd29dCkURLd6JKrFgtmcDTZ",
                "shards": [
                    0,
                    1,
                    2,
                    3
                ],
                "stake": "43022911372752167869222722706568"
            },
            {
                "account_id": "bzam6yjpnfnxsdmjf6pw.poolv1.near",
                "is_slashed": false,
                "num_expected_blocks": 829,
                "num_expected_chunks": 3304,
                "num_produced_blocks": 829,
                "num_produced_chunks": 3304,
                "public_key": "ed25519:2ZJqaaCAisK4u8E2i611zFfvNmrvevovnU3M7SpGHkLY",
                "shards": [
                    0,
                    1,
                    2,
                    3
                ],
                "stake": "23384108164579494784911789880683"
            }
        ],
        "epoch_height": 1186,
        "epoch_start_height": 61055481,
        "next_fishermen": [],
        "next_validators": [
            {
                "account_id": "staked.poolv1.near",
                "public_key": "ed25519:3JBVXqenru2ErAM1kHQ8qfd29dCkURLd6JKrFgtmcDTZ",
                "shards": [
                    0,
                    1,
                    2,
                    3
                ],
                "stake": "43031952664819829506377623789143"
            },
            {
                "account_id": "bzam6yjpnfnxsdmjf6pw.poolv1.near",
                "public_key": "ed25519:2ZJqaaCAisK4u8E2i611zFfvNmrvevovnU3M7SpGHkLY",
                "shards": [
                    0,
                    1,
                    2,
                    3
                ],
                "stake": "23388809235408741833747580128034"
            }
        ],
        "prev_epoch_kickout": [
            {
                "account_id": "lavenderfive.poolv1.near",
                "reason": {
                    "NotEnoughBlocks": {
                        "expected": 13,
                        "produced": 0
                    }
                }
            }
        ]
    }
}
```
