---
description: >-
  Example code for the staking validator REST method. Complete guide on how to
  use staking validator REST method in GetBlock Web3 documentation.
---

# staking validator

Returns a single validator by its operator address.

## Endpoint

```http
GET /cosmos/staking/v1beta1/validators/{validator_addr}
```

## Path Parameters

| Parameter       | Type   | Description                                    |
| --------------- | ------ | ---------------------------------------------- |
| validator\_addr | string | Validator operator address (cosmosvaloper1...) |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/staking/v1beta1/validators/stablevaloper1qg5ega6dykkxc307y25pecuufrjkxkagqe8n2y5"
```
{% endcode %}

## Response

```json
{
    "validator": {
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
}
```

## Response Fields

| Field                       | Type   | Description                |
| --------------------------- | ------ | -------------------------- |
| validator.operator\_address | string | Validator operator address |
| validator.status            | string | Bond status                |
| validator.tokens            | string | Bonded tokens in ausdt0    |
| validator.commission        | object | Commission rates           |

## Use Cases

* **Validator detail**: Show one validator's stats

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
