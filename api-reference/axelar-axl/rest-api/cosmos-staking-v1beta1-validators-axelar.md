---
description: >-
  Example code for the cosmos/staking/v1beta1/validators REST method. Complete
  guide on how to use cosmos/staking/v1beta1/validators REST method in GetBlock
  Web3 documentation.
---

# /cosmos/staking/v1beta1/validators - Axelar

Returns the paginated set of staking validators, each with its operator address, commission, and bonded status.

## Endpoint

```http
GET /cosmos/staking/v1beta1/validators
```

## Query Parameters

| Parameter        | Type   | Description                             |
| ---------------- | ------ | --------------------------------------- |
| status           | string | Filter by BOND\_STATUS\_BONDED/UNBONDED |
| pagination.limit | string | Max results                             |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/staking/v1beta1/validators"
```
{% endcode %}

## Response

```json
{
    "validators": [
        {
            "operator_address": "axelarvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "status": "BOND_STATUS_BONDED",
            "tokens": "5000000000000",
            "description": {
                "moniker": "Validator One"
            },
            "commission": {
                "commission_rates": {
                    "rate": "0.050000000000000000"
                }
            }
        }
    ],
    "pagination": {
        "total": "100"
    }
}
```

## Response Fields

| Field                           | Type   | Description                                |
| ------------------------------- | ------ | ------------------------------------------ |
| validators                      | array  | Validators with tokens, status, commission |
| validators\[].operator\_address | string | Operator address (axelarvaloper1…)         |

## Use Cases

* **Staking UIs**: List validators
* **Commission Compare**: Sort by rate
* **Analytics**: Aggregate bonded stake

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
