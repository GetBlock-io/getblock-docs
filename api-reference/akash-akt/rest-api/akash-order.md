---
description: >-
  Example code for the akash/market/v1beta4/orders/info REST method. Complete
  guide on how to use akash/market/v1beta4/orders/info REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta4/orders/info - Akash

Returns a single marketplace order by its full id (owner, dseq, gseq, oseq).

## Endpoint

```http
GET /akash/market/v1beta4/orders/info
```

## Query Parameters

| Parameter | Type   | Description         |
| --------- | ------ | ------------------- |
| id.owner  | string | Owner               |
| id.dseq   | string | Deployment sequence |
| id.gseq   | string | Group sequence      |
| id.oseq   | string | Order sequence      |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/market/v1beta4/orders/info?id.owner=akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p&id.dseq=12345678&id.gseq=1&id.oseq=1"
```
{% endcode %}

## Response

```json
{
    "order": {
        "order_id": {
            "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "dseq": "12345678",
            "gseq": 1,
            "oseq": 1
        },
        "state": "open",
        "spec": {}
    }
}
```

## Response Fields

| Field | Type   | Description               |
| ----- | ------ | ------------------------- |
| order | object | Order id, state, and spec |

## Use Cases

* **Marketplace**: Render an order page

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
