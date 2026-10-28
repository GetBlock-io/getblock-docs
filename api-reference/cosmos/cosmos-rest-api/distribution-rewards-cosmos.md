---
description: >-
  Example code for the distribution rewards REST method. Complete guide on how
  to use the distribution rewards REST method in GetBlock Web3 documentation.
---

# distribution rewards - Cosmos

Returns the outstanding staking rewards for a delegator, per validator and in total.

## Endpoint

```http
GET /cosmos/distribution/v1beta1/delegators/{delegator_addr}/rewards
```

## Path Parameters

| Parameter       | Type   | Description                           |
| --------------- | ------ | ------------------------------------- |
| delegator\_addr | string | Bech32 delegator address (cosmos1...) |

## Example

{% code overflow="wrap" %}
```bash
export COSMOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${COSMOS_REST}cosmos/distribution/v1beta1/delegators/cosmos1xv9tklw7d82sezh9haa573wufgy59vmwe6xxe5/rewards"
```
{% endcode %}

## Response

```json
{
    "rewards": [
        {
            "validator_address": "cosmosvaloper1xv9tklw7d82sezh9haa573wufgy59vmwnp5s7g",
            "reward": [
                {
                    "denom": "uatom",
                    "amount": "12500.000000000000000000"
                }
            ]
        }
    ],
    "total": [
        {
            "denom": "uatom",
            "amount": "12500.000000000000000000"
        }
    ]
}
```

## Response Fields

| Field   | Type  | Description                                            |
| ------- | ----- | ------------------------------------------------------ |
| rewards | array | Per-validator reward entries for the delegator         |
| total   | array | Total outstanding rewards across validators, per denom |

## Use Cases

* **Rewards UI**: Show claimable staking rewards
* **Auto-compound**: Read rewards before a claim/restake

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
