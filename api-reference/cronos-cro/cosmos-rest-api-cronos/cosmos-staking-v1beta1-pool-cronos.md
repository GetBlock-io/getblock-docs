---
description: >-
  Example code for the cosmos/staking/v1beta1/pool REST method. Complete guide
  on how to use cosmos/staking/v1beta1/pool REST method in GetBlock Web3
  documentation.
---

# /cosmos/staking/v1beta1/pool - Cronos

Returns the staking pool totals: the amount of tokens currently bonded and not bonded across the whole network.

## Endpoint

```http
GET /cosmos/staking/v1beta1/pool
```

## Example

{% code overflow="wrap" %}
```bash
export CRONOS_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${CRONOS_REST}cosmos/staking/v1beta1/pool"
```
{% endcode %}

## Response

```json
{
    "pool": {
        "not_bonded_tokens": "1000000000000000000000000",
        "bonded_tokens": "15000000000000000000000000000"
    }
}
```

## Response Fields

| Field                    | Type   | Description                       |
| ------------------------ | ------ | --------------------------------- |
| pool.bonded\_tokens      | string | Total tokens bonded to validators |
| pool.not\_bonded\_tokens | string | Total tokens not bonded           |

## Use Cases

* **Staking Ratio**: Compute the network staking ratio
* **Analytics**: Track bonded supply over time
* **Dashboards**: Show total staked on a stats page

## Error Handling

| Error                     | Message        | Description                                       |
| ------------------------- | -------------- | ------------------------------------------------- |
| 500 / internal            | Internal error | The node failed to return the pool                |
| 403 / RBAC: access denied | Access denied  | The GetBlock access token is missing or incorrect |
