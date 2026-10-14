---
description: >-
  Example code for the estimate gas price JSON-RPC method. Complete guide on how
  to use estimate gas price JSON-RPC in GetBlock Web3 documentation.
---

# estimate gas price - Movement

Returns the network's current gas unit price estimate, with deprioritized and prioritized tiers, so a client can set a competitive `gas_unit_price`.

## Endpoint

```http
GET /v1/estimate_gas_price
```

## Example

{% code overflow="wrap" %}
```bash
export MOVEMENT_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${MOVEMENT_REST}v1/estimate_gas_price"
```
{% endcode %}

## Response

```json
{
    "deprioritized_gas_estimate": 90,
    "gas_estimate": 100,
    "prioritized_gas_estimate": 150
}
```

## Response Fields

| Field                        | Type    | Description                               |
| ---------------------------- | ------- | ----------------------------------------- |
| gas\_estimate                | integer | Recommended gas unit price                |
| prioritized\_gas\_estimate   | integer | Higher price for faster inclusion         |
| deprioritized\_gas\_estimate | integer | Lower price for cheaper, slower inclusion |

## Use Cases

* **Fee Estimation**: Set a competitive gas unit price
* **Cost Control**: Choose a priority tier

## Error Handling

| Error                     | Message       | Description                                                 |
| ------------------------- | ------------- | ----------------------------------------------------------- |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                |
| 404 / Not found           | Not found     | The account, block, transaction, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect           |
