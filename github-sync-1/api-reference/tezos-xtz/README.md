---
description: >-
  GetBlock provides fast and reliable access to Tezos nodes via REST API.
  Connect to the Tezos network without running your own infrastructure.
---

# Tezos (XTZ)

Tezos is a self-amending, energy-efficient Layer-1 blockchain that upgrades itself through on-chain governance rather than hard forks. It uses a Liquid Proof-of-Stake consensus (Tenderbake) for fast, deterministic finality, and runs smart contracts written in Michelson. The native token is XTZ (tez), denominated in mutez (1 XTZ = 1,000,000 mutez). Accounts are either implicit (tz1/tz2/tz3/tz4) or originated smart contracts (KT1). GetBlock exposes Tezos through the Octez node RPC — a REST (HTTP + JSON) interface where every resource is addressed by URL path, with reads over GET and simulation, forging, and injection over POST.

### Key Features

* **Self-Amending L1**: On-chain governance upgrades the protocol without hard forks
* **Liquid Proof-of-Stake**: Tenderbake consensus gives deterministic finality within a couple of blocks
* **REST RPC**: The Octez node exposes a path-addressed REST API — no JSON-RPC envelope
* **Michelson Contracts**: Originated KT1 contracts with on-chain code, storage, views, and big maps
* **mutez Precision**: Balances and amounts are in mutez (1 XTZ = 1,000,000 mutez)
* **Block-Pinned Queries**: Most reads take a block identifier (head, a level, or a hash) for historical state

{% hint style="info" %}
_TECHNICAL DISCLAIMER: AUTHORITATIVE REST API SPECIFICATION._

_GetBlock's API reference documentation is provided exclusively for informational purposes and to optimize the developer experience. The canonical specification is the Octez node RPC, published at_ [_tezos.gitlab.io_](https://tezos.gitlab.io/shell/rpc.html)_; protocol and developer documentation is at_ [_docs.tezos.com_](https://docs.tezos.com/)_._
{% endhint %}

### Network Information

| Property          | Value                                              |
| ----------------- | -------------------------------------------------- |
| Network Name      | Tezos Mainnet                                      |
| Chain ID          | NetXdQprcVkpaWU                                    |
| Native Currency   | XTZ (1 XTZ = 1,000,000 mutez)                      |
| Consensus         | Liquid Proof-of-Stake (Tenderbake)                 |
| Contract Language | Michelson                                          |
| Address Formats   | tz1 / tz2 / tz3 / tz4 (implicit), KT1 (originated) |
| Finality          | Deterministic (\~2 blocks)                         |

### Base URL

{% tabs %}
{% tab title="Frankfurt, Germany" %}
```bash
https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
```
{% endtab %}
{% endtabs %}

### Quickstart

The Octez RPC is REST, not JSON-RPC: the resource is chosen by the URL path appended to your GetBlock endpoint. Reads are GET; simulation, forging, pre-apply, and injection are POST with a JSON body. Most block-scoped paths accept `head`, a level, or a block hash in place of `{block}`.

{% tabs %}
{% tab title="curl" %}
{% code overflow="wrap" %}
```bash
export TEZOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/
curl "${TEZOS_REST}chains/main/blocks/head/header" | jq .level
```
{% endcode %}
{% endtab %}

{% tab title="Javascript" %}
{% code overflow="wrap" %}
```js
const axios = require('axios');

let config = {
  method: 'get',
  maxBodyLength: Infinity,
  url: 'https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>/chains/main/blocks/head',
  headers: { }
};

axios.request(config)
.then((response) => {
  console.log(JSON.stringify(response.data));
})
.catch((error) => {
  console.log(error);
});

```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code overflow="wrap" %}
```python
import requests

url = "https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>/chains/main/blocks/head"

payload = {}
headers = {}

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)

```
{% endcode %}
{% endtab %}

{% tab title="Go" %}
{% code overflow="wrap" %}
```go
package main

import (
  "fmt"
  "net/http"
  "io"
)

func main() {

  url := "https://shared.eu-central-1.getblock.io/<ACCESS_TOKEN>/chains/main/blocks/head"
  method := "GET"

  client := &http.Client {
  }
  req, err := http.NewRequest(method, url, nil)

  if err != nil {
    fmt.Println(err)
    return
  }
  res, err := client.Do(req)
  if err != nil {
    fmt.Println(err)
    return
  }
  defer res.Body.Close()

  body, err := io.ReadAll(res.Body)
  if err != nil {
    fmt.Println(err)
    return
  }
  fmt.Println(string(body))
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

### Response

{% code overflow="wrap" %}
```bash
{
    "protocol": "PsUshuai9QapM5TGj1JpuVGkdxz5GykdnEvS6Rh8SUVrARvZLCY",
    "chain_id": "NetXdQprcVkpaWU",
    "hash": "BL6eya89cPvUAZm7s9GvVcd91Vov3S3ajSJBLc6sbA2EBaWpPyg",
    "header": {
        "level": 15267099,
        "proto": 25,
        "predecessor": "BL3VYmBvHWyCBrdeNDMR5TVEec3iV4aPetrELbi4BqcZdNv5MiY",
        "timestamp": "2026-10-07T08:19:19Z",
        "validation_pass": 4,
        "operations_hash": "LLoZoMZTJE26zxcTNFLrFVoCvA4B1ktiGtCHHZNzpXkczT8EUEQHC",
        "fitness": [
            "02",
            "00e8f51b",
            "",
            "ffffffff",
            "00000000"
        ],
        "context": "CoVtEnUd1WPbmZuioV8G5NYAThbEyn24gN9LGPMSwKgoAJcvrSFR",
        "payload_hash": "vh3ZsPWFeEhJ4hBrjKv4RkCJgU877MQgzJdTGNgwn5BnetLsYTeP",
        "payload_round": 0,
        "proof_of_work_nonce": "f60628535b780200",
        "liquidity_baking_toggle_vote": "pass",
        "signature": "sigVSYzT9CqeXtp7TvMT2xQS93nJ1HADDbZACB6VMrazimi18M9orVzBDW85jtW9R3Remy2rjNgtBV7LswJcJRmsXpZTkQbe"
    },
    "metadata": {
        "protocol": "PsUshuai9QapM5TGj1JpuVGkdxz5GykdnEvS6Rh8SUVrARvZLCY",
        "next_protocol": "PsUshuai9QapM5TGj1JpuVGkdxz5GykdnEvS6Rh8SUVrARvZLCY",
        "test_chain_status": {
            "status": "not_running"
        },
        "max_operations_ttl": 600,
        "max_operation_data_length": 32768,
        "max_block_header_length": 289,
        "max_operation_list_length": [
            {
                "max_size": 4194304,
                "max_op": 2048
            }
        ],
        "proposer": "tz1RCFbB9GpALpsZtu6J58sb74dm8qe6XBzv",
        "baker": "tz1RCFbB9GpALpsZtu6J58sb74dm8qe6XBzv",
        "level_info": {
            "level": 15267099,
            "level_position": 15267098,
            "cycle": 1375,
            "cycle_position": 12410,
            "expected_commitment": false
        },
        "voting_period_info": {
            "voting_period": {
                "index": 183,
                "kind": "proposal",
                "start_position": 15067488
            },
            "position": 199610,
            "remaining": 1989
        },
        "nonce_hash": null,
        "deactivated": [],
        "balance_updates": [
            {
                "kind": "accumulator",
                "category": "block fees",
                "change": "-1536",
                "origin": "block"
            }
        ],
        "liquidity_baking_toggle_ema": 1444916504,
        "implicit_operations_results": [],
        "proposer_consensus_key": "tz1RCFbB9GpALpsZtu6J58sb74dm8qe6XBzv",
        "baker_consensus_key": "tz1RCFbB9GpALpsZtu6J58sb74dm8qe6XBzv",
        "consumed_milligas": "593000",
        "dal_attestation": "0",
        "all_bakers_attest_activation_level": null,
        "attestations": {
            "total_committee_power": "7000",
            "threshold": "4667",
            "recorded_power": "6998"
        },
        "preattestations": null
    },
    "operations": [
        [
            {
                "protocol": "PsUshuai9QapM5TGj1JpuVGkdxz5GykdnEvS6Rh8SUVrARvZLCY",
                "chain_id": "NetXdQprcVkpaWU",
                "hash": "oojZ8vVkrVTSty9z8L28zccu3d7PUQZ2vtspLtXpuyeNPGuTBiw",
                "branch": "BMaNdb3QTLMvNFx3qfKWp8HeX8mD6FHGMQt6nzhfTteRRa4GRx9",
                "contents": [
                    {
                        "kind": "attestation_with_dal",
                        "slot": 6689,
                        "level": 15267098,
                        "round": 0,
                        "block_payload_hash": "vh1oSJeYR1nEyjf9bxeBHCLXqfgdDv6hX2xrw8nmsG9KS4X1nrZb",
                        "dal_attestation": "0",
                        "metadata": {
                            "delegate": "tz1iS7kehbR1CKAd38HrssP38BBz8GC9Zhy4",
                            "consensus_power": {
                                "slots": 1,
                                "baking_power": null
                            },
                            "consensus_key": "tz1iS7kehbR1CKAd38HrssP38BBz8GC9Zhy4"
                        }
                    }
                ],
                "signature": "sigVvswXqK7wMW1xKmC6N9BkLFjBnkTyoFzgbwUTd1vzV4s2VG6Kdb1jZdUQzsrvQthSM8ere565xTinSo6vf28RgiqdcTPz"
            },
            {
                "protocol": "PsUshuai9QapM5TGj1JpuVGkdxz5GykdnEvS6Rh8SUVrARvZLCY",
                "chain_id": "NetXdQprcVkpaWU",
                "hash": "ooFubmx1JuXfJRBckAzpfCy774EDPDiLeV6CPdXq97AbziZ9PgE",
                "branch": "BMaNdb3QTLMvNFx3qfKWp8HeX8mD6FHGMQt6nzhfTteRRa4GRx9",
                "contents": [
                    {
                        "kind": "attestations_aggregate",
                        "consensus_content": {
                            "level": 15267098,
                            "round": 0,
                            "block_payload_hash": "vh1oSJeYR1nEyjf9bxeBHCLXqfgdDv6hX2xrw8nmsG9KS4X1nrZb"
                        },
                        "committee": [
                            {
                                "slot": 0,
                                "dal_attestation": "0"
                            }
                        ],
                        "metadata": {
                            "committee": [
                                {
                                    "delegate": "tz1aRoaRhSpRYvFdyvgWLL6TGyRoGF51wDjM",
                                    "consensus_pkh": "tz4LDetQVRvHihaVVpX7YG4mAu7EbYz4HMm4",
                                    "consensus_power": {
                                        "slots": 451,
                                        "baking_power": null
                                    }
                                }
                                }
                            ],
                            "total_consensus_power": {
                                "slots": 2328,
                                "baking_power": null
                            }
                        }
                    }
                ],
                "signature": "BLsigAAEFa8K8KJKQgMEV53HEYUwKBhfwiibXBDkHyk34GcR9VrTfc7ZZkmXAeKd3vKBirfYS8Rr8Ubvu8ipBZvw1PdvrLu8tLoic9UCTKTB4BZ5aqAheLny9ET3c3g1sFMufASnubLEZQ"
            }
        ],
        [],
        [],
        [
            {
                "protocol": "PsUshuai9QapM5TGj1JpuVGkdxz5GykdnEvS6Rh8SUVrARvZLCY",
                "chain_id": "NetXdQprcVkpaWU",
                "hash": "oosNvdBu9ijuGRyUvfEEa1VYH8kBgQFqhVeLNSV3v73NNn8UUXN",
                "branch": "BMaNdb3QTLMvNFx3qfKWp8HeX8mD6FHGMQt6nzhfTteRRa4GRx9",
                "contents": [
                    {
                        "kind": "smart_rollup_add_messages",
                        "source": "tz3Vx9cZepdhWrbmRqftux1jeMc1NaQ5iDKZ",
                        "fee": "1536",
                        "counter": "171843932",
                        "gas_limit": "593",
                        "storage_limit": "0",
                        "message": [
                            "0074f8952e7a287d78e8dceec67547bd00a278abbf03f904c0b90454f9045101a0aa6163341ad451b0868f65e84ff8372ba5d7ce160235ef2427616955213fff7ac0f90422b9041f01f9041b83498fc4843b9aca00834c4b4094a2cca359c43839040cf3d230deb1689ab8db2dac80b903afc14c9204000000000000000000000000000000000000000000000000000001a"
                        ],
                        "metadata": {
                            "balance_updates": [
                                {
                                    "kind": "contract",
                                    "contract": "tz3Vx9cZepdhWrbmRqftux1jeMc1NaQ5iDKZ",
                                    "change": "-1536",
                                    "origin": "block"
                                },
                                {
                                    "kind": "accumulator",
                                    "category": "block fees",
                                    "change": "1536",
                                    "origin": "block"
                                }
                            ],
                            "operation_result": {
                                "status": "applied",
                                "consumed_milligas": "492878"
                            }
                        }
                    }
                ],
                "signature": "sigv8VxEQnzGMzHx6LP9B1QJ8XiodaGzpZsSvZiZcJUqMZKkYzW5x61yPDFC3z43AsSv8G1Gvq4eJdcZ8BRM3mSiqcqEekZn"
            }
        ]
    ]
}
```
{% endcode %}

## Available REST Endpoints

### Chain & Node Info

| Endpoint                    | Method | Description                                                                                     |
| --------------------------- | ------ | ----------------------------------------------------------------------------------------------- |
| version                     | GET    | Returns the Octez node version, network protocol, and commit information                        |
| chain-id                    | GET    | Returns the base58-encoded chain identifier of the main chain                                   |
| blocks                      | GET    | Returns a list of the most recent block hashes on the main chain, most recent first             |
| monitor-heads _(dedicated)_ | GET    | Opens a streaming response that emits each new block header as it is appended to the main chain |

### Blocks

| Endpoint               | Method | Description                                                                                                                                                      |
| ---------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| block-head             | GET    | Returns the complete block at the chain head — its hash, header, metadata, and all operations                                                                    |
| block-hash             | GET    | Returns just the hash of the given block                                                                                                                         |
| block-header           | GET    | Returns the full header of the given block: level, protocol, predecessor, timestamp, fitness, operations hash, and the baker's signature                         |
| block-header-shell     | GET    | Returns the protocol-agnostic shell portion of the block header — level, predecessor, timestamp, fitness, and operations hash — without protocol-specific fields |
| block-metadata         | GET    | Returns protocol-level metadata for the block: the active and next protocol, baker, consumed gas, voting period, and balance updates (rewards, deposits, burns)  |
| block-operations       | GET    | Returns all operations included in the block, grouped by validation pass (endorsements, votes, anonymous, and managers)                                          |
| block-operation-hashes | GET    | Returns only the operation hashes included in the block, grouped by validation pass — a lightweight alternative to fetching full operation contents              |

### Context — Constants & Accounts

| Endpoint             | Method | Description                                                                                                                               |
| -------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| constants            | GET    | Returns the active protocol's constants — block time, blocks per cycle, minimal stake, hard gas and storage limits, and reward parameters |
| contract             | GET    | Returns the full state of an account or contract: balance, counter, delegate, and (for originated KT1 contracts) script and storage       |
| contract-balance     | GET    | Returns the spendable balance of an account or contract in mutez (1 XTZ = 1,000,000 mutez)                                                |
| contract-counter     | GET    | Returns the operation counter (nonce) of an implicit account                                                                              |
| contract-manager-key | GET    | Returns the public key of an implicit account, or null if the account has never revealed its key                                          |

### Context — Contracts & Storage

| Endpoint             | Method | Description                                                                                                                                                 |
| -------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| contract-storage     | GET    | Returns the current storage of an originated (KT1) smart contract as a Michelson expression (Micheline JSON)                                                |
| contract-script      | GET    | Returns the Michelson code and initial storage of an originated (KT1) contract as a Micheline expression                                                    |
| contract-entrypoints | GET    | Returns the named entrypoints of an originated (KT1) contract and their parameter types, so a caller knows how to build a valid transaction to the contract |
| big-map-value        | GET    | Returns a single value from a big map by its packed-key hash (script expression)                                                                            |

### Staking & Delegates

| Endpoint                 | Method | Description                                                                                                                                                       |
| ------------------------ | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| delegate                 | GET    | Returns the full staking state of a delegate (baker): full and staking balance, delegated contracts, frozen deposits, grace period, and whether it is deactivated |
| delegate-staking-balance | GET    | Returns a delegate's total staking balance in mutez — its own stake plus all balances delegated to it — which determines baking and attestation rights            |

### Governance

| Endpoint             | Method | Description                                                                                                                                          |
| -------------------- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| votes-current-period | GET    | Returns the current governance (amendment) voting period: its kind (proposal, exploration, cooldown, promotion, adoption), index, and start position |

### Mempool & Transactions

| Endpoint                   | Method | Description                                                                                                                                                        |
| -------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| mempool-pending-operations | GET    | Returns operations currently in the node's mempool, classified as applied, refused, branch\_refused, branch\_delayed, or outdated                                  |
| run-operation              | POST   | Simulates (dry-runs) a signed operation against the current context without broadcasting it, returning the operation results including consumed gas and any errors |
| run-script-view            | POST   | Executes an on-chain view of a smart contract and returns its result, without creating a transaction                                                               |
| forge-operations           | POST   | Serializes (forges) an operation's JSON representation into the binary hex payload that must be signed before injection                                            |
| preapply-operations        | POST   | Pre-applies a signed operation to predict its result and metadata before injection, surfacing errors and balance updates                                           |
| inject-operation           | POST   | Broadcasts a signed, forged operation to the network and returns its operation hash                                                                                |

## Support

For technical support and questions:

* Support: [support@getblock.io](mailto:support@getblock.io)

## See Also

* [Octez RPC Reference](https://tezos.gitlab.io/shell/rpc.html)
* [Tezos Developer Documentation](https://docs.tezos.com/)
* [TzKT Explorer](https://tzkt.io/)
* [Tezos Website](https://tezos.com/)
