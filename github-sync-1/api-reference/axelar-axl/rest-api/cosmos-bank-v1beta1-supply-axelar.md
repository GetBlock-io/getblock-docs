---
description: >-
  Example code for the cosmos/bank/v1beta1/supply REST method. Complete guide on
  how to use cosmos/bank/v1beta1/supply REST method in GetBlock Web3
  documentation.
---

# cosmos/bank/v1beta1/supply - Axelar

Returns the total supply of every denom on the chain, paginated.

## Endpoint

```
GET /cosmos/bank/v1beta1/supply
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/bank/v1beta1/supply"
```
{% endcode %}

## Response

```json
{
    "supply": [
        {
            "denom": "uaxl",
            "amount": "388539008000000"
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field  | Type  | Description            |
| ------ | ----- | ---------------------- |
| supply | array | Total supply per denom |

## Use Cases

* **Tokenomics**: Read AXL supply
* **Analytics**: Track supply over time
* **Explorers**: Show supply stats

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
