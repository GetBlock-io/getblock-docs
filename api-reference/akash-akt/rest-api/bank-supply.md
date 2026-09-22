---
description: >-
  Example code for the cosmos/bank/v1beta1/supply REST method. Complete guide
  on how to use cosmos/bank/v1beta1/supply REST method in GetBlock Web3
  documentation.
---

# /cosmos/bank/v1beta1/supply - Akash

Returns the total supply of every denom on the chain, paginated.

## Endpoint

```http
GET /cosmos/bank/v1beta1/supply
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/bank/v1beta1/supply?pagination.limit=2"
```
{% endcode %}

## Response

```json
{
    "supply": [
        {
            "denom": "ibc/011C19FB6113363238248C55B985A92C0A0CAF9709162EAB838EACB6A629E6AA",
            "amount": "1950000"
        }
    ],
    "pagination": {
        "next_key": "aWJjLzA0Qzk0NDAwMDZCNjU4Q0RDOEFGQUE4OTlCM0EzQ0NGQ0JENUE4NDY1N0U2MjdEQkI2MjIzM0I3RUZCRUI5NTg=",
        "total": "0"
    }
}
```

## Response Fields

| Field  | Type  | Description            |
| ------ | ----- | ---------------------- |
| supply | array | Total supply per denom |

## Use Cases

* **Tokenomics**: Read AKT supply
* **Analytics**: Track supply over time
* **Explorers**: Show supply stats

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
