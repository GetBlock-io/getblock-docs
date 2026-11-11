---
description: >-
  Example code for the staking pool REST method. Complete guide on how to use
  staking pool REST method in GetBlock Web3 documentation.
---

# staking pool - Stable

Returns the staking pool: the total bonded and not-bonded token amounts.

## Endpoint

```http
GET /cosmos/staking/v1beta1/pool
```

## Example

{% code overflow="wrap" %}
```bash
export STABLE_REST=https://shared.ap-southeast-1.getblock.io/<ACCESS-TOKEN>/

curl "${STABLE_REST}cosmos/staking/v1beta1/pool"
```
{% endcode %}

## Response

```json
{
    "pool": {
        "not_bonded_tokens": "20000000000000",
        "bonded_tokens": "280000000000000"
    }
}
```

## Response Fields

| Field                    | Type   | Description                            |
| ------------------------ | ------ | -------------------------------------- |
| pool.bonded\_tokens      | string | Total bonded (staked) tokens in ausdt0 |
| pool.not\_bonded\_tokens | string | Total not-bonded tokens in ausdt0      |

## Use Cases

* **Staking ratio**: Compute bonded vs. total supply
* **Network health**: Track staking participation

## Error Handling

| Error                     | Message       | Description                                                  |
| ------------------------- | ------------- | ------------------------------------------------------------ |
| 400 / Bad request         | Bad request   | A path, query, or body parameter was invalid                 |
| 404 / Not found           | Not found     | The account, validator, proposal, or resource does not exist |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect            |
