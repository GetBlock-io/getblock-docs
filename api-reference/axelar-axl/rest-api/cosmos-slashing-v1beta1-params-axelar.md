---
description: >-
  Example code for the cosmos/slashing/v1beta1/params REST method. Complete
  guide on how to use cosmos/slashing/v1beta1/params REST method in GetBlock
  Web3 documentation.
---

# /cosmos/slashing/v1beta1/params - Axelar

Returns the slashing module parameters (signed-blocks window, min signed, slash fractions).

## Endpoint

```http
GET /cosmos/slashing/v1beta1/params
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/slashing/v1beta1/params"
```
{% endcode %}

## Response

```json
{
    "params": {
        "signed_blocks_window": "10000",
        "min_signed_per_window": "0.050000000000000000",
        "slash_fraction_double_sign": "0.050000000000000000",
        "slash_fraction_downtime": "0.000100000000000000"
    }
}
```

## Response Fields

| Field  | Type   | Description         |
| ------ | ------ | ------------------- |
| params | object | Slashing parameters |

## Use Cases

* **Validator Ops**: Read downtime/double-sign penalties

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
