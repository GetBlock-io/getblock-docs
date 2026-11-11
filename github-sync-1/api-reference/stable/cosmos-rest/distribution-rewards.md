---
description: >-
  Example code for the distribution rewards REST method. Complete guide on how
  to use distribution rewards REST method in GetBlock Web3 documentation.
---

# distribution rewards - Stable

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
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/distribution/v1beta1/delegators/stable1qg5ega6dykkxc307y25pecuufrjkxkagq9k7m4h/rewards"
```
{% endcode %}

## Response

```json
{
    "rewards": [
        {
            "validator_address": "stablevaloper1qg5ega6dykkxc307y25pecuufrjkxkagqe8n2y5",
            "reward": [
                {
                    "denom": "ausdt0",
                    "amount": "12500.000000000000000000"
                }
            ]
        }
    ],
    "total": [
        {
            "denom": "ausdt0",
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
