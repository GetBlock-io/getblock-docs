---
description: >-
  Example code for the staking validators JSON-RPC method. Complete guide on how
  to use staking validators JSON-RPC in GetBlock Web3 documentation.
---

# /cosmos/staking/v1beta1/validators - TAC

Returns all staking validators, optionally filtered by status, paginated.

## Endpoint

```http
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
export TAC_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TAC_REST}cosmos/staking/v1beta1/validators"
```
{% endcode %}

## Response

```json
{
    "validators": [
        {
            "operator_address": "tacvaloper1ql8dru5dgm6gtg3hcdq9lds3q3fnk3s6xhj2p7",
            "status": "BOND_STATUS_BONDED",
            "tokens": "5000000000000000000000",
            "description": {
                "moniker": "tac-validator"
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

| Field                           | Type   | Description                                                             |
| ------------------------------- | ------ | ----------------------------------------------------------------------- |
| validators                      | array  | Validators with operator address, status, bonded tokens, and commission |
| validators\[].operator\_address | string | Validator operator address (tacvaloper1...)                             |
| validators\[].tokens            | string | Total bonded tokens in the base denomination                            |

## Use Cases

* **Validator list**: Build a staking UI
* **Delegation**: Rank validators by stake or commission

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
