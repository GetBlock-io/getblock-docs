---
description: >-
  Example code for the staking validator JSON-RPC method. Complete guide on how
  to use staking validator JSON-RPC in GetBlock Web3 documentation.
---

# /cosmos/staking/v1beta1/validators/{validator\_addr} - TAC

Returns a single validator by its operator address.

## Endpoint

```http
GET /cosmos/staking/v1beta1/validators/{validator_addr}
```

## Path Parameters

| Parameter       | Type   | Description                                 |
| --------------- | ------ | ------------------------------------------- |
| validator\_addr | string | Validator operator address (tacvaloper1...) |

## Example

{% code overflow="wrap" %}
```bash
export TAC_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TAC_REST}cosmos/staking/v1beta1/validators/tacvaloper1ql8dru5dgm6gtg3hcdq9lds3q3fnk3s6xhj2p7"
```
{% endcode %}

## Response

```json
{
    "validator": {
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
}
```

## Response Fields

| Field                       | Type   | Description                            |
| --------------------------- | ------ | -------------------------------------- |
| validator.operator\_address | string | Validator operator address             |
| validator.status            | string | Bond status                            |
| validator.tokens            | string | Bonded tokens in the base denomination |
| validator.commission        | object | Commission rates                       |

## Use Cases

* **Validator detail**: Show one validator's stats
* **Pre-delegation**: Inspect commission and status before delegating

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
