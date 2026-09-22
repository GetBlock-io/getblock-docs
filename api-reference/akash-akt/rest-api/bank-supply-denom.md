---
description: >-
  Example code for the cosmos/bank/v1beta1/supply/by_denom REST method.
  Complete guide on how to use cosmos/bank/v1beta1/supply/by_denom REST method
  in GetBlock Web3 documentation.
---

# /cosmos/bank/v1beta1/supply/by\_denom - Akash

Returns the total supply of one denom.

## Endpoint

```http
GET /cosmos/bank/v1beta1/supply/by_denom
```

## Query Parameters

| Parameter | Type   | Description    |
| --------- | ------ | -------------- |
| denom     | string | Denom to query |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/bank/v1beta1/supply/by_denom?denom=uakt"
```
{% endcode %}

## Response

```json
{
    "amount": {
        "denom": "uakt",
        "amount": "388539008000000"
    }
}
```

## Response Fields

| Field  | Type   | Description               |
| ------ | ------ | ------------------------- |
| amount | object | Total supply of the denom |

## Use Cases

* **Tokenomics**: Read AKT supply directly

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
