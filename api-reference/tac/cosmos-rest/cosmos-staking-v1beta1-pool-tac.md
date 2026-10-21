---
description: >-
  Example code for the staking pool JSON-RPC method. Complete guide on how to
  use staking pool JSON-RPC in GetBlock Web3 documentation.
---

# /cosmos/staking/v1beta1/pool - TAC

Returns the staking pool: the total bonded and not-bonded token amounts.

## Endpoint

```http
GET /cosmos/staking/v1beta1/pool
```

## Example

{% code overflow="wrap" %}
```bash
export TAC_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${TAC_REST}cosmos/staking/v1beta1/pool"
```
{% endcode %}

## Response

```json
{
    "pool": {
        "not_bonded_tokens": "20000000000000000000000",
        "bonded_tokens": "480000000000000000000000"
    }
}
```

## Response Fields

| Field                    | Type   | Description                                           |
| ------------------------ | ------ | ----------------------------------------------------- |
| pool.bonded\_tokens      | string | Total bonded (staked) tokens in the base denomination |
| pool.not\_bonded\_tokens | string | Total not-bonded tokens in the base denomination      |

## Use Cases

* **Staking ratio**: Compute bonded vs. total supply
* **Network health**: Track staking participation

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
