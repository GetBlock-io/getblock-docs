---
description: >-
  Example code for the validators json-rpc method. Сomplete guide on how to use
  validators json-rpc in GetBlock.io Web3 documentation.
---

# validators - NEAR Protocol

This method provides detailed information about **current, upcoming, and previous validators** on the NEAR blockchain.

## Supported Networks

- Mainnet

## Parameters

- None

## Request Example

**Base URL**
```bash
https://go.getblock.io/<ACCESS_TOKEN>
```

**Example(cURL)**

```bash
curl -X POST https://go.getblock.io/<ACCESS_TOKEN> \
-H "Content-Type: application/json" \
-d '{"jsonrpc": "2.0",
"method": "validators",
"params": [null],
"id": "getblock.io"}'
```


## Response Example

```json
{
    "jsonrpc": "2.0",
    "result": {
        "current_fishermen": [],
        "current_proposals": [
            {
                "account_id": "001mrcoin.pool.near",
                "public_key": "ed25519:47bmEkWhm3pxPaWenBCEhM7xbAeohuiYVNNRjnWoWhxw",
                "stake": "77069033005549257593709607276",
                "validator_stake_struct_version": "V1"
            }
        ],
        "current_validators": [
            {
                "account_id": "croutondigital.pool.near",
                "is_slashed": false,
                "num_expected_blocks": 0,
                "num_expected_chunks": 0,
                "num_expected_chunks_per_shard": [],
                "num_expected_endorsements": 30863,
                "num_expected_endorsements_per_shard": [
                    3517,
                    3480,
                    3377,
                    3428,
                    3413,
                    3405,
                    3469,
                    3389,
                    3385
                ],
                "num_produced_blocks": 0,
                "num_produced_chunks": 0,
                "num_produced_chunks_per_shard": [],
                "num_produced_endorsements": 29610,
                "num_produced_endorsements_per_shard": [
                    3435,
                    3423,
                    3055,
                    3374,
                    3312,
                    3305,
                    3033,
                    3328,
                    3345
                ],
                "public_key": "ed25519:HFtbBTFQo46Jkd5Dvwu7nY5ZoAG7piVGpHneSqpCNQpH",
                "shards": [],
                "shards_endorsed": [
                    1,
                    4,
                    5,
                    6,
                    7,
                    8,
                    9,
                    10,
                    11
                ],
                "stake": "11003694780164446245528847844"
            }
        ],
        "epoch_height": 3703,
        "epoch_start_height": 169789917,
        "next_fishermen": [],
        "next_validators": [
            {
                "account_id": "bitcoinsuisse.poolv1.near",
                "public_key": "ed25519:Cy2sboVqjDk6d3d2A2AJZBdFvokjk7sjZpYATLjcQSCj",
                "shards": [
                    4
                ],
                "stake": "1379544217119337898196119177738"
            },
        ],
        "prev_epoch_kickout": [
            {
                "account_id": "sparkpool.poolv1.near",
                "reason": {
                    "NotEnoughChunkEndorsements": {
                        "expected": 43172,
                        "produced": 0
                    }
                }
            },
            {
                "account_id": "stakeseeker.poolv1.near",
                "reason": {
                    "NotEnoughChunkEndorsements": {
                        "expected": 43172,
                        "produced": 0
                    }
                }
            },
            {
                "account_id": "validatrium.poolv1.near",
                "reason": {
                    "NotEnoughChunkEndorsements": {
                        "expected": 43172,
                        "produced": 0
                    }
                }
            }
        ]
    },
    "id": "getblock.io"
}
```

## Response Parameters Definition

| **Field**                                                  | **Type** | **Description**                                                                |
| ---------------------------------------------------------- | -------- | ------------------------------------------------------------------------------ |
| **current_proposals**                                      | array    | Information about validator proposals for the next epoch.                      |
| **current_proposals.account_id**                           | string   | Identifier of the validator account proposing to join.                         |
| **current_proposals.public_key**                           | string   | Public key of the validator.                                                   |
| **current_proposals.stake**                                | string   | Amount of tokens staked (in yoctoNEAR).                                        |
| **current_proposals.validator_stake_struct_version**       | string   | Version of the validator stake struct.                                         |
| **current_validators**                                     | array    | Information about validators currently participating in the epoch.             |
| **current_validators.account_id**                          | string   | Identifier of the current validator.                                           |
| **current_validators.public_key**                          | string   | Public key of the validator.                                                   |
| **current_validators.is_slashed**                          | boolean  | Indicates if the validator has been slashed.                                   |
| **current_validators.num_expected_chunks_per_shard**       | array    | Number of expected chunks per shard.                                           |
| **current_validators.num_produced_chunks_per_shard**       | array    | Number of chunks produced per shard.                                           |
| **current_validators.num_produced_endorsements**           | integer  | Total endorsements the validator produced.                                     |
| **current_validators.num_produced_endorsements_per_shard** | array    | Endorsements produced per shard.                                               |
| **current_validators.shards_endorsed**                     | array    | List of shards endorsed by this validator.                                     |
| **current_validators.num_expected_endorsements**           | integer  | Number of endorsements expected.                                               |
| **current_validators.stake**                               | string   | Total tokens staked by the validator.                                          |
| **current_validators.shards**                              | array    | Shard IDs the validator operates on.                                           |
| **current_validators.num_expected_blocks**                 | integer  | Expected blocks the validator should produce.                                  |
| **current_validators.num_expected_chunks**                 | integer  | Expected chunks the validator should produce.                                  |
| **current_validators.num_produced_blocks**                 | integer  | Blocks actually produced by the validator.                                     |
| **current_validators.num_produced_chunks**                 | integer  | Chunks actually produced by the validator.                                     |
| **next_validators**                                        | array    | List of validators scheduled for the next epoch.                               |
| **next_validators.account_id**                             | string   | Identifier for the upcoming validator.                                         |
| **next_validators.public_key**                             | string   | Public key of the validator.                                                   |
| **next_validators.stake**                                  | string   | Stake amount for the next validator.                                           |
| **next_validators.shards**                                 | array    | Shard assignments for the next validator.                                      |
| **current_fishermen**                                      | array    | Information about active fishermen (participants monitoring network behavior). |
| **current_fishermen.account_id**                           | string   | Account ID of the current fisherman.                                           |
| **current_fishermen.public_key**                           | string   | Public key of the fisherman.                                                   |
| **current_fishermen.stake**                                | string   | Amount of tokens staked by the fisherman.                                      |
| **next_fishermen**                                         | array    | Information about fishermen in the next epoch.                                 |
| **next_fishermen.account_id**                              | string   | Account ID of the next fisherman.                                              |
| **next_fishermen.public_key**                              | string   | Public key of the fisherman.                                                   |
| **next_fishermen.stake**                                   | string   | Amount staked by the fisherman.                                                |
| **prev_epoch_kickout**                                     | array    | List of validators removed in the previous epoch and their reasons.            |
| **prev_epoch_kickout.account_id**                          | string   | Identifier of the validator kicked out.                                        |
| **prev_epoch_kickout.reason**                              | object   | Reason for removal.                                                            |
| **prev_epoch_kickout.reason.NotEnoughChunks.expected**     | integer  | Expected number of chunks.                                                     |
| **prev_epoch_kickout.reason.NotEnoughChunks.produced**     | integer  | Produced number of chunks.                                                     |
| **epoch_start_height**                                     | integer  | The block height at which the epoch started.                                   |
| **epoch_height**                                           | integer  | The height (number) of the current epoch.                                      |


## Use Cases

- Monitor current validator activity and **block production performance**.
- Track upcoming validators joining the next epoch.
- Support staking dashboards and validator ranking systems.


## Code Example

**Node(Axios)**

```js
import axios from 'axios';
let data = JSON.stringify({
  "jsonrpc": "2.0",
  "method": "validators",
  "params": [
    null
  ],
  "id": "getblock.io"
});
let config = {
  method: 'post',
  maxBodyLength: Infinity,
  url: 'https://go.getblock.io/<ACCESS_TOKEN>',
  headers: { 
    'Content-Type': 'application/json'
  },
  data : data
};
axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});
```

**Python(Requests)**

```python
import requests
import json
url = "https://go.getblock.io/<ACCESS_TOKEN>"
payload = json.dumps({
  "jsonrpc": "2.0",
  "method": "validators",
  "params": [
    None
  ],
  "id": "getblock.io"
})
headers = {
  'Content-Type': 'application/json'
}
response = requests.request("POST", url, headers=headers, data=payload)
print(response.text)
```

## Error Handling

| **HTTP Code**                 | **Error Message**                | **Description**                                             |
| ----------------------------- | -------------------------------- | ----------------------------------------------------------- |
| **500 Internal Server Error** | `Failed to fetch validator data` | Internal node error while retrieving validator information. |
| **403 Forbidden**   | `RBAC: access denied`   | The Getblock access token is missing or incorrect                             |

## Integration with Web3

By integrating `/validators` with dApp, developers can:

- Create staking analytics dashboards.
- Display real-time validator metrics to delegators.
- Maintain transparent governance by tracking validator proposals.
