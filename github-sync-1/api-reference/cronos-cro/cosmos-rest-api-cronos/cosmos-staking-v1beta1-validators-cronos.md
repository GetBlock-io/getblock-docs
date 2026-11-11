---
description: >-
  Example code for the cosmos/staking/v1beta1/validators REST method. Complete
  guide on how to use cosmos/staking/v1beta1/validators REST method in GetBlock
  Web3 documentation.
---

# /cosmos/staking/v1beta1/validators - Cronos

Returns the paginated set of staking validators (operators), each with its operator address, commission, and bonded status. Filter by status with a query parameter.

## Endpoint

```http
GET /cosmos/staking/v1beta1/validators
```

## Query Parameters

| Parameter        | Type   | Description                                           |
| ---------------- | ------ | ----------------------------------------------------- |
| status           | string | Filter by BOND\_STATUS\_BONDED / UNBONDED / UNBONDING |
| pagination.limit | string | Max results to return                                 |

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/staking/v1beta1/validators"
```
{% endcode %}

## Response

```json
{
    "validators": [
        {
            "operator_address": "crcvaloper1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
            "status": "BOND_STATUS_BONDED",
            "tokens": "5000000000000000000000000",
            "delegator_shares": "5000000000000000000000000.0",
            "description": {
                "moniker": "Validator One"
            },
            "commission": {
                "commission_rates": {
                    "rate": "0.100000000000000000"
                }
            }
        }
    ],
    "pagination": {
        "total": "30"
    }
}
```

## Response Fields

| Field                           | Type   | Description                                             |
| ------------------------------- | ------ | ------------------------------------------------------- |
| validators                      | array  | Validator operators with tokens, status, and commission |
| validators\[].operator\_address | string | Operator address (crcvaloper1…)                         |
| validators\[].tokens            | string | Total bonded tokens delegated to the validator          |

## Use Cases

* **Staking UIs**: List validators for delegation
* **Commission Compare**: Sort validators by commission rate
* **Analytics**: Aggregate bonded stake across the set

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / internal            | Internal error | The node failed to return validators              |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
