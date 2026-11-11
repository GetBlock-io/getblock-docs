---
description: >-
  Example code for the staking validators REST method. Complete guide on how to
  use staking validators REST method in GetBlock Web3 documentation.
---

# staking validators - Stable

Returns all staking validators, optionally filtered by status, paginated.

## Endpoint

```
GET /cosmos/staking/v1beta1/validators
```

## Query Parameters

| Parameter        | Type   | Description                                 |
| ---------------- | ------ | ------------------------------------------- |
| status           | string | Filter by status, e.g. BOND\_STATUS\_BONDED |
| pagination.limit | string | Max results per page                        |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/staking/v1beta1/validators"
```
{% endcode %}

## Response

```json
{
    "validators": [
        {
            "operator_address": "stablevaloper1qg5ega6dykkxc307y25pecuufrjkxkagqe8n2y5",
            "status": "BOND_STATUS_BONDED",
            "tokens": "5000000000000",
            "description": {
                "moniker": "cosmos-validator"
            },
            "commission": {
                "commission_rates": {
                    "rate": "0.050000000000000000"
                }
            }
        }
    ],
    "pagination": {
        "total": "180"
    }
}
```

## Response Fields

| Field                           | Type   | Description                                                             |
| ------------------------------- | ------ | ----------------------------------------------------------------------- |
| validators                      | array  | Validators with operator address, status, bonded tokens, and commission |
| validators\[].operator\_address | string | Validator operator address (cosmosvaloper1...)                          |
| validators\[].tokens            | string | Total bonded tokens in ausdt0                                           |

## Use Cases

* **Validator list**: Build a staking UI
* **Delegation**: Rank validators by stake or commission

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
