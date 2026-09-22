---
description: >-
  Example code for the cosmos/base/tendermint/v1beta1/blocks/latest REST
  method. Complete guide on how to use
  cosmos/base/tendermint/v1beta1/blocks/latest REST method in GetBlock Web3
  documentation.
---

# /cosmos/base/tendermint/v1beta1/blocks/latest - Akash

Returns the latest committed block via the Cosmos base-tendermint service, with the block id, header, and transaction data.

## Endpoint

```http
GET /cosmos/base/tendermint/v1beta1/blocks/latest
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/base/tendermint/v1beta1/blocks/latest"
```
{% endcode %}

## Response

```json
{
    "block_id": {
        "hash": "mBGPwqUUpWXU6I5YpGOM4A3HvdHJCxpfynbuI7epM3M=",
        "part_set_header": {
            "total": 1,
            "hash": "2I/tEdMhmgJQVDMc0bWCU0m50ZBeFJPhEXgobsM5Q8E="
        }
    },
    "block": {
        "header": {
            "version": {
                "block": "11",
                "app": "0"
            },
            "chain_id": "akashnet-2",
            "height": "28742381",
            "time": "2026-09-22T18:17:41.262600678Z",
            "last_block_id": {
                "hash": "0YgS6R5SN+XZx3Zo4RxmijtM332074fauIlMizIHpnA=",
                "part_set_header": {
                    "total": 1,
                    "hash": "1cPGFmYtbdhnNywolx4CaSOMoPnEZcRYOLN4TdOxNqU="
                }
            },
            "last_commit_hash": "wkJbdkOaaKfJOKOKJv6mqEEFVSqzwskbHcJDyyUrEAo=",
            "data_hash": "G92gk486iceRvZ1jPzv6TpoDHVgxjpkeITvPBr4r6+Y=",
            "validators_hash": "RO+btMxni6gOn+atN4nd2+/BLM5/KBptk6Ss2ALMYlI=",
            "next_validators_hash": "RO+btMxni6gOn+atN4nd2+/BLM5/KBptk6Ss2ALMYlI=",
            "consensus_hash": "BICRvH3cKD93v7+R1zxE2ljD34qcvIZ0Bdi389qtoi8=",
            "app_hash": "LllC+v0ZWS/cHyAfSqM2fPP2lZBs/oMtdu4uGAvzyEI=",
            "last_results_hash": "MQeXV9iCQjnEDYzEn3m2cT/TaYYRAKaH2aC/VNld0Zg=",
            "evidence_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "proposer_address": "6rkde0AhPhToUKkNfCjmYkZt18Y="
        },
        "data": {
            "txs": [
                "CogBCmcKLC9ha2FzaC5kZXBsb3ltZW50LnYxYmV0YTQuTXNnQ2xvc2VEZXBsb3ltZW50EjcKNQosYWth..."
            ]
        },
        "evidence": {
            "evidence": []
        },
        "last_commit": {
            "height": "28742380",
            "round": 0,
            "block_id": {
                "hash": "0YgS6R5SN+XZx3Zo4RxmijtM332074fauIlMizIHpnA=",
                "part_set_header": {
                    "total": 1,
                    "hash": "1cPGFmYtbdhnNywolx4CaSOMoPnEZcRYOLN4TdOxNqU="
                }
            },
            "signatures": [
                {
                    "block_id_flag": "BLOCK_ID_FLAG_COMMIT",
                    "validator_address": "sYUtF/pmtTgqj3cHJcxbIos1d1A=",
                    "timestamp": "2026-09-22T18:17:41.253803591Z",
                    "signature": "AKppdq3t+00TM2Mlx+ttj4dLcvvadmh4e0HNd/rf08tjmxrqA7pvqF7w+TLudmY/exv3d5ZAPg0/42iQoG2RDQ=="
                }
            ]
        }
    },
    "sdk_block": {
        "header": {
            "version": {
                "block": "11",
                "app": "0"
            },
            "chain_id": "akashnet-2",
            "height": "28742381",
            "time": "2026-09-22T18:17:41.262600678Z",
            "last_block_id": {
                "hash": "0YgS6R5SN+XZx3Zo4RxmijtM332074fauIlMizIHpnA=",
                "part_set_header": {
                    "total": 1,
                    "hash": "1cPGFmYtbdhnNywolx4CaSOMoPnEZcRYOLN4TdOxNqU="
                }
            },
            "last_commit_hash": "wkJbdkOaaKfJOKOKJv6mqEEFVSqzwskbHcJDyyUrEAo=",
            "data_hash": "G92gk486iceRvZ1jPzv6TpoDHVgxjpkeITvPBr4r6+Y=",
            "validators_hash": "RO+btMxni6gOn+atN4nd2+/BLM5/KBptk6Ss2ALMYlI=",
            "next_validators_hash": "RO+btMxni6gOn+atN4nd2+/BLM5/KBptk6Ss2ALMYlI=",
            "consensus_hash": "BICRvH3cKD93v7+R1zxE2ljD34qcvIZ0Bdi389qtoi8=",
            "app_hash": "LllC+v0ZWS/cHyAfSqM2fPP2lZBs/oMtdu4uGAvzyEI=",
            "last_results_hash": "MQeXV9iCQjnEDYzEn3m2cT/TaYYRAKaH2aC/VNld0Zg=",
            "evidence_hash": "47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=",
            "proposer_address": "akashvalcons1a2u3676qyylpf6zs4yxhc28xvfrxm47xz7hr7n"
        },
        "data": {
            "txs": [
                "CogBCmcKLC9ha2FzaC5kZXBsb3ltZW50LnYxYmV0YTQuTXNnQ2xvc2VEZXBsb3ltZW50EjcKNQosYWth..."
            ]
        },
        "evidence": {
            "evidence": []
        },
        "last_commit": {
            "height": "28742380",
            "round": 0,
            "block_id": {
                "hash": "0YgS6R5SN+XZx3Zo4RxmijtM332074fauIlMizIHpnA=",
                "part_set_header": {
                    "total": 1,
                    "hash": "1cPGFmYtbdhnNywolx4CaSOMoPnEZcRYOLN4TdOxNqU="
                }
            },
            "signatures": [
                {
                    "block_id_flag": "BLOCK_ID_FLAG_COMMIT",
                    "validator_address": "sYUtF/pmtTgqj3cHJcxbIos1d1A=",
                    "timestamp": "2026-09-22T18:17:41.253803591Z",
                    "signature": "AKppdq3t+00TM2Mlx+ttj4dLcvvadmh4e0HNd/rf08tjmxrqA7pvqF7w+TLudmY/exv3d5ZAPg0/42iQoG2RDQ=="
                }
            ]
        }
    }
}
```

## Response Fields

| Field                  | Type   | Description         |
| ---------------------- | ------ | ------------------- |
| block.header.height    | string | Latest block height |
| block.header.chain\_id | string | Chain id            |

## Use Cases

* **Chain Tip**: Read the latest height
* **Following**: Poll the latest block
* **Explorers**: Render the latest block

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
