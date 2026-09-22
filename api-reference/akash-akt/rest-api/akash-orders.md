---
description: >-
  Example code for the akash/market/v1beta4/orders/list REST method. Complete
  guide on how to use akash/market/v1beta4/orders/list REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta4/orders/list - Akash

Returns marketplace orders. When a deployment's group is open, the market creates an order that providers bid on.

## Endpoint

```http
GET /akash/market/v1beta4/orders/list
```

## Query Parameters

| Parameter        | Type   | Description              |
| ---------------- | ------ | ------------------------ |
| filters.owner    | string | Filter by owner address  |
| filters.state    | string | open, matched, or closed |
| pagination.limit | string | Max results              |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/market/v1beta4/orders/list"
```
{% endcode %}

## Response

```json
{
    "orders": [
        {
            "order_id": {
                "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
                "dseq": "12345678",
                "gseq": 1,
                "oseq": 1
            },
            "state": "open"
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field               | Type   | Description                                |
| ------------------- | ------ | ------------------------------------------ |
| orders              | array  | Orders with order\_id and state            |
| orders\[].order\_id | object | Owner/dseq/gseq/oseq identifying the order |

## Use Cases

* **Marketplace**: Track open orders
* **Providers**: Find orders to bid on
* **Indexing**: Ingest order state

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
