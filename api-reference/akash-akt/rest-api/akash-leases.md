# akash leases

Returns leases — accepted bids that bind a deployment to a provider. A lease is an active rental of compute from a provider.

## Endpoint

```http
GET /akash/market/v1beta4/leases/list
```

## Query Parameters

| Parameter        | Type   | Description        |
| ---------------- | ------ | ------------------ |
| filters.owner    | string | Filter by owner    |
| filters.provider | string | Filter by provider |
| filters.state    | string | active or closed   |
| pagination.limit | string | Max results        |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/market/v1beta4/leases/list"
```
{% endcode %}

## Response

```json
{
    "leases": [
        {
            "lease": {
                "lease_id": {
                    "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
                    "provider": "akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
                    "dseq": "12345678"
                },
                "state": "active",
                "price": {
                    "denom": "uakt",
                    "amount": "1.5"
                }
            }
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field                 | Type   | Description                             |
| --------------------- | ------ | --------------------------------------- |
| leases                | array  | Leases with lease\_id, state, and price |
| leases\[].lease.state | string | active or closed                        |

## Use Cases

* **Active Rentals**: List a user's active leases
* **Billing**: Read lease price for cost tracking
* **Providers**: Track a provider's leases

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
