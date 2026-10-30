---
description: >-
  Example code for the bank supply REST method. Complete guide on how to use
  bank supply REST method in GetBlock Web3 documentation.
---

# bank supply

Returns the total supply of all denominations on the chain, paginated.

## Endpoint

```
GET /cosmos/bank/v1beta1/supply
```

## Query Parameters

| Parameter        | Type   | Description          |
| ---------------- | ------ | -------------------- |
| pagination.limit | string | Max results per page |

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/bank/v1beta1/supply"
```
{% endcode %}

## Response

```json
{
    "supply": [
        {
            "denom": "ausdt0",
            "amount": "390000000000000"
        }
    ],
    "pagination": {
        "next_key": null,
        "total": "1"
    }
}
```

## Response Fields

| Field            | Type   | Description                                  |
| ---------------- | ------ | -------------------------------------------- |
| supply           | array  | Total supply per denom                       |
| supply\[].denom  | string | Base denomination                            |
| supply\[].amount | string | Total minted amount in the base denomination |

## Use Cases

* **Supply tracking**: Read total USDT0 supply
* **Metrics**: Feed supply data into dashboards

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
