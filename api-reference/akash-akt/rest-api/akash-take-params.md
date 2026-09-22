---
description: >-
  Example code for the akash/take/v1beta3/params REST method. Complete guide
  on how to use akash/take/v1beta3/params REST method in GetBlock Web3
  documentation.
---

# /akash/take/v1beta3/params - Akash

Returns the marketplace take parameters — the protocol fee taken from lease payments, per denom.

## Endpoint

```http
GET /akash/take/v1beta3/params
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/take/v1beta3/params"
```
{% endcode %}

## Response

```json
{
    "params": {
        "default_take_rate": 20,
        "denom_take_rates": [
            {
                "denom": "uakt",
                "rate": 20
            }
        ]
    }
}
```

## Response Fields

| Field  | Type   | Description                       |
| ------ | ------ | --------------------------------- |
| params | object | Take-rate configuration per denom |

## Use Cases

* **Fees**: Read the marketplace fee
* **Economics**: Model provider net revenue

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
