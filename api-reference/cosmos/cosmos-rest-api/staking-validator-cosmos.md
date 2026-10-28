---
description: >-
  Example code for the staking validator REST method. Complete guide on how to
  use the staking validator REST method in GetBlock Web3 documentation.
---

# staking validator - Cosmos

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
export COSMOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${COSMOS_REST}cosmos/staking/v1beta1/validators/cosmosvaloper1xv9tklw7d82sezh9haa573wufgy59vmwnp5s7g"
```
{% endcode %}

## Response

```json
{
    "validator": {
        "operator_address": "cosmosvaloper1xv9tklw7d82sezh9haa573wufgy59vmwnp5s7g",
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
| validator.tokens            | string | Bonded tokens in uatom     |
| validator.commission        | object | Commission rates           |

## Use Cases

* **Validator detail**: Show one validator's stats

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
