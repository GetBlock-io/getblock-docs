---
description: >-
  Example code for the cosmos/staking/v1beta1/pool REST method. Complete guide
  on how to use cosmos/staking/v1beta1/pool REST method in GetBlock Web3
  documentation.
---

# /cosmos/staking/v1beta1/pool - Axelar

Returns the staking pool totals: bonded and not-bonded tokens across the network.

## Endpoint

```http
GET /cosmos/staking/v1beta1/pool
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/staking/v1beta1/pool"
```
{% endcode %}

## Response

```json
{
    "pool": {
        "not_bonded_tokens": "5000000000000",
        "bonded_tokens": "190000000000000"
    }
}
```

## Response Fields

| Field                    | Type   | Description         |
| ------------------------ | ------ | ------------------- |
| pool.bonded\_tokens      | string | Total bonded tokens |
| pool.not\_bonded\_tokens | string | Total not bonded    |

## Use Cases

* **Staking Ratio**: Compute the staking ratio
* **Analytics**: Track bonded supply
* **Dashboards**: Show total staked

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
