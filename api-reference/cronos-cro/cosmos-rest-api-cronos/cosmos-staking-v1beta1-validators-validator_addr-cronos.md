---
description: >-
  Example code for the cosmos/staking/v1beta1/validators/{validator_addr} REST
  method. Complete guide on how to use
  cosmos/staking/v1beta1/validators/{validator_addr} REST method in GetBlock
  Web3 documentation.
---

# /cosmos/staking/v1beta1/validators/{validator\_addr} - Cronos

Returns one staking validator by its operator address, with full commission, description, and bonded-token details.

## Endpoint

```http
GET /cosmos/staking/v1beta1/validators/{validator_addr}
```

## Path Parameters

| Parameter       | Type   | Description                     |
| --------------- | ------ | ------------------------------- |
| validator\_addr | string | Operator address (crcvaloper1…) |

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/staking/v1beta1/validators/crcvaloper1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j"
```
{% endcode %}

## Response

```json
{
    "validator": {
        "operator_address": "crcvaloper1p9zq0lr6h6n5s2wk9m3v7t4x8c6b2d0f1g3h5j",
        "status": "BOND_STATUS_BONDED",
        "tokens": "5000000000000000000000000",
        "description": {
            "moniker": "Validator One",
            "website": "https://example.com"
        },
        "commission": {
            "commission_rates": {
                "rate": "0.100000000000000000",
                "max_rate": "0.200000000000000000"
            }
        }
    }
}
```

## Response Fields

| Field                | Type   | Description                               |
| -------------------- | ------ | ----------------------------------------- |
| validator.tokens     | string | Total bonded tokens                       |
| validator.commission | object | Commission rate, max rate, and max change |
| validator.status     | string | Bonded / unbonding / unbonded status      |

## Use Cases

* **Validator Pages**: Render a validator's detail view
* **Delegation**: Confirm a validator is bonded before delegating
* **Monitoring**: Watch a validator's bonded stake

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 404 / not found           | Not found     | No validator with that operator address           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
