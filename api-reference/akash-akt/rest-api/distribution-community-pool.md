# distribution community pool

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
            "denom": "uakt",
            "amount": "5000000000.0"
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
