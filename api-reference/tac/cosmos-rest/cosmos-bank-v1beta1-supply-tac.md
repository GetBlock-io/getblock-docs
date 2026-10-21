---
description: >-
  Example code for the bank supply JSON-RPC method. Complete guide on how to use
  bank supply JSON-RPC in GetBlock Web3 documentation.
---

# /cosmos/bank/v1beta1/supply - TAC

Returns the total supply of all denominations on the chain, paginated.

## Endpoint

```http
GET /cosmos/bank/v1beta1/supply
```

## Query Parameters

| Parameter        | Type   | Description          |
| ---------------- | ------ | -------------------- |
| pagination.limit | string | Max results per page |

## Example

{% code overflow="wrap" %}
```bash
export TAC_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TAC_REST}cosmos/bank/v1beta1/supply"
```
{% endcode %}

## Response

```json
{
    "supply": [
        {
            "denom": "atac",
            "amount": "1000000000000000000000000000"
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

* **Supply tracking**: Read circulating/total supply per denom
* **Metrics**: Feed supply data into dashboards

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
