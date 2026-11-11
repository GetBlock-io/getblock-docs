---
description: >-
  Example code for the cosmos/mint/v1beta1/inflation REST method. Complete
  guide on how to use cosmos/mint/v1beta1/inflation REST method in GetBlock
  Web3 documentation.
---

# /cosmos/mint/v1beta1/inflation - Akash

Returns the current minting inflation rate.

## Endpoint

```http
GET /cosmos/mint/v1beta1/inflation
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/mint/v1beta1/inflation"
```
{% endcode %}

## Response

```json
{
    "inflation": "0.040000000000000000"
}
```

## Response Fields

| Field     | Type   | Description           |
| --------- | ------ | --------------------- |
| inflation | string | Annual inflation rate |

## Use Cases

* **Tokenomics**: Read the inflation rate

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
