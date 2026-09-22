---
description: >-
  Example code for the akash/market/v1beta4/bids/list REST method. Complete
  guide on how to use akash/market/v1beta4/bids/list REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta4/bids/list - Akash

Returns provider bids on marketplace orders. A bid is a provider's price offer to host a deployment's workload.

## Endpoint

```
GET /akash/market/v1beta4/bids/list
```

## Query Parameters

| Parameter        | Type   | Description                |
| ---------------- | ------ | -------------------------- |
| filters.owner    | string | Filter by deployment owner |
| filters.provider | string | Filter by provider address |
| filters.state    | string | open, active, or closed    |
| pagination.limit | string | Max results                |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/market/v1beta4/bids/list"
```
{% endcode %}

## Response

```json
{
    "bids": [
        {
            "bid": {
                "bid_id": {
                    "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
                    "provider": "akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
                    "dseq": "12345678"
                },
                "state": "open",
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

| Field             | Type   | Description                         |
| ----------------- | ------ | ----------------------------------- |
| bids              | array  | Bids with bid\_id, state, and price |
| bids\[].bid.price | object | Provider's offered price per block  |

## Use Cases

* **Price Discovery**: Compare provider bids
* **Marketplace UIs**: Show bids for an order
* **Providers**: Track a provider's bids

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
