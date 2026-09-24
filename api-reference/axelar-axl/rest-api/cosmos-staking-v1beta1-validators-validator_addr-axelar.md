---
description: >-
  Example code for the cosmos/staking/v1beta1/validators/{validator_addr} REST
  method. Complete guide in GetBlock Web3 documentation.
---

# /cosmos/staking/v1beta1/validators/{validator\_addr} - Axelar

Returns one staking validator by operator address.

## Endpoint

```http
GET /cosmos/staking/v1beta1/validators/{validator_addr}
```

## Path Parameters

| Parameter       | Type   | Description                        |
| --------------- | ------ | ---------------------------------- |
| validator\_addr | string | Operator address (axelarvaloper1…) |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/staking/v1beta1/validators/axelarvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
```
{% endcode %}

## Response

```json
{
    "validator": {
        "operator_address": "axelarvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
        "status": "BOND_STATUS_BONDED",
        "tokens": "5000000000000",
        "commission": {
            "commission_rates": {
                "rate": "0.050000000000000000"
            }
        }
    }
}
```

## Response Fields

| Field     | Type   | Description       |
| --------- | ------ | ----------------- |
| validator | object | Validator details |

## Use Cases

* **Validator Pages**: Render a validator

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
