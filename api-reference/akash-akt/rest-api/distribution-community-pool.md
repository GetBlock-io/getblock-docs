---
description: >-
  Example code for the cosmos/distribution/v1beta1/community_pool REST method.
  Complete guide on how to use cosmos/distribution/v1beta1/community_pool REST
  method in GetBlock Web3 documentation.
---

# /cosmos/distribution/v1beta1/community\_pool - Akash

Returns the coins held in the community pool.

## Endpoint

```
GET /cosmos/distribution/v1beta1/community_pool
```

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}cosmos/distribution/v1beta1/community_pool"
```
{% endcode %}

## Response

```json
{
    "pool": [
        {
            "denom": "ibc/170C677610AC31DF0904FFE09CD3B5C657492170E7E52372E48756B71E56F2F1",
            "amount": "263775119968.000000000000000000"
        }
    ]
}
```

## Response Fields

| Field | Type  | Description          |
| ----- | ----- | -------------------- |
| pool  | array | Community pool coins |

## Use Cases

* **Treasury**: Read the community pool

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
