---
description: >-
  Example code for the distribution rewards JSON-RPC method. Complete guide on
  how to use distribution rewards JSON-RPC in GetBlock Web3 documentation.
---

# /cosmos/distribution/v1beta1/delegators/{delegator\_addr}/rewards - TAC

Returns the outstanding staking rewards for a delegator, per validator and in total.

## Endpoint

```http
GET /cosmos/distribution/v1beta1/delegators/{delegator_addr}/rewards
```

## Path Parameters

| Parameter       | Type   | Description                        |
| --------------- | ------ | ---------------------------------- |
| delegator\_addr | string | Bech32 delegator address (tac1...) |

## Example

{% code overflow="wrap" %}
```bash
export TAC_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TAC_REST}cosmos/distribution/v1beta1/delegators/tac1ql8dru5dgm6gtg3hcdq9lds3q3fnk3s6k9h8f0/rewards"
```
{% endcode %}

## Response

```json
{
    "rewards": [
        {
            "validator_address": "tacvaloper1ql8dru5dgm6gtg3hcdq9lds3q3fnk3s6xhj2p7",
            "reward": [
                {
                    "denom": "atac",
                    "amount": "125000000000000000.000000000000000000"
                }
            ]
        }
    ],
    "total": [
        {
            "denom": "atac",
            "amount": "125000000000000000.000000000000000000"
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
