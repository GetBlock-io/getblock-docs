---
description: >-
  Example code for the queryNetwork_genesisConfiguration JSON-RPC method.
  Complete guide on how to use queryNetwork_genesisConfiguration JSON-RPC in
  GetBlock Web3 documentation.
---

# queryNetwork\_genesisConfiguration - Cardano

This method returns the genesis configuration for a given era. The configuration holds the constants an era was initialized with, such as the network magic and slot parameters.

## Parameters

| Parameter | Type   | Required | Description                                                    |
| --------- | ------ | -------- | -------------------------------------------------------------- |
| era       | string | No       | The era to read genesis for: byron, shelley, alonzo, or conway |

## Request

{% tabs %}
{% tab title="cURL" %}
{% code overflow="wrap" %}
```bash
curl --location --request POST 'https://go.getblock.io/<ACCESS-TOKEN>/' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jsonrpc": "2.0",
    "method": "queryNetwork/genesisConfiguration",
    "params": {
        "era": "shelley"
    },
    "id": "getblock.io"
}'
```
{% endcode %}
{% endtab %}

{% tab title="JavaScript" %}
{% code title="example.js" %}
```javascript
const response = await fetch(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({"jsonrpc": "2.0", "method": "queryNetwork/genesisConfiguration", "params": {"era": "shelley"}, "id": "getblock.io"})
    }
);
console.log(await response.json());
```
{% endcode %}
{% endtab %}

{% tab title="Python" %}
{% code title="example.py" %}
```python
import requests

response = requests.post(
    'https://go.getblock.io/<ACCESS-TOKEN>/',
    headers={'Content-Type': 'application/json'},
    json={"jsonrpc": "2.0", "method": "queryNetwork/genesisConfiguration", "params": {"era": "shelley"}, "id": "getblock.io"}
)

print(response.json())
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Response

{% code overflow="wrap" %}
```json
{
    "jsonrpc": "2.0",
    "method": "queryNetwork/genesisConfiguration",
    "result": {
        "era": "shelley",
        "startTime": "2017-09-23T21:44:51Z",
        "networkMagic": 764824073,
        "network": "mainnet",
        "activeSlotsCoefficient": "1/20",
        "securityParameter": 2160,
        "epochLength": 432000,
        "slotsPerKesPeriod": 129600,
        "maxKesEvolutions": 62,
        "slotLength": {
            "milliseconds": 1000
        },
        "updateQuorum": 5,
        "maxLovelaceSupply": 45000000000000000,
        "initialParameters": {
            "minFeeCoefficient": 44,
            "minFeeConstant": {
                "ada": {
                    "lovelace": 155381
                }
            },
            "maxBlockBodySize": {
                "bytes": 65536
            },
            "maxBlockHeaderSize": {
                "bytes": 1100
            },
            "maxTransactionSize": {
                "bytes": 16384
            },
            "stakeCredentialDeposit": {
                "ada": {
                    "lovelace": 2000000
                }
            },
            "stakePoolDeposit": {
                "ada": {
                    "lovelace": 500000000
                }
            },
            "stakePoolRetirementEpochBound": 18,
            "desiredNumberOfStakePools": 150,
            "stakePoolPledgeInfluence": "3/10",
            "minStakePoolCost": {
                "ada": {
                    "lovelace": 340000000
                }
            },
            "monetaryExpansion": "3/1000",
            "treasuryExpansion": "1/5",
            "federatedBlockProductionRatio": "1/1",
            "extraEntropy": "neutral",
            "minUtxoDepositConstant": {
                "ada": {
                    "lovelace": 1000000
                }
            },
            "minUtxoDepositCoefficient": 0,
            "version": {
                "major": 2,
                "minor": 0
            }
        },
        "initialDelegates": [
            {
                "issuer": {
                    "id": "162f94554ac8c225383a2248c245659eda870eaa82d0ef25fc7dcd82"
                },
                "delegate": {
                    "id": "4485708022839a7b9b8b639a939c85ec0ed6999b5b6dc651b03c43f6",
                    "vrfVerificationKeyHash": "aba81e764b71006c515986bf7b37a72fbb5554f78e6775f08e384dbd572a4b32"
                }
            },
            {
                "issuer": {
                    "id": "b9547b8a57656539a8d9bc42c008e38d9c8bd9c8adbb1e73ad529497"
                },
                "delegate": {
                    "id": "855d6fc1e54274e331e34478eeac8d060b0b90c1f9e8a2b01167c048",
                    "vrfVerificationKeyHash": "66d5167a1f426bd1adcc8bbf4b88c280d38c148d135cb41e3f5a39f948ad7fcc"
                }
            },
            {
                "issuer": {
                    "id": "f7b341c14cd58fca4195a9b278cce1ef402dc0e06deb77e543cd1757"
                },
                "delegate": {
                    "id": "69ae12f9e45c0c9122356c8e624b1fbbed6c22a2e3b4358cf0cb5011",
                    "vrfVerificationKeyHash": "6394a632af51a32768a6f12dac3485d9c0712d0b54e3f389f355385762a478f2"
                }
            }
        ],
        "initialFunds": {},
        "initialStakePools": {
            "stakePools": {},
            "delegators": {}
        }
    },
    "id": "getblock.io"
}
```
{% endcode %}

## Response Parameters

| Field             | Type    | Description                           |
| ----------------- | ------- | ------------------------------------- |
| networkMagic      | integer | Network magic identifying the network |
| network           | string  | Network name, mainnet or testnet      |
| epochLength       | integer | Number of slots in an epoch           |
| securityParameter | integer | The security parameter k              |
| maxLovelaceSupply | integer | Maximum total supply in lovelace      |

## Use Cases

* **Parameter Bootstrapping**: Read genesis constants when initializing a client
* **Network Detection**: Confirm the network magic before submitting transactions
* **Slot Math**: Use epoch length and slot length for time conversions
* **Supply Modeling**: Read the maximum lovelace supply for issuance models

## Error Handling

| Error Code | Message           | Description                                          |
| ---------- | ----------------- | ---------------------------------------------------- |
| -32602     | Invalid params    | A required field is missing or has the wrong type    |
| -32000     | Query unavailable | The query is not available in the current ledger era |
| -32603     | Internal error    | The node failed to answer the query                  |
