---
description: >-
  Example code for the akash/market/v1beta4/leases/info REST method. Complete
  guide on how to use akash/market/v1beta4/leases/info REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta4/leases/info - Akash

Returns a single lease by its full id, plus its escrow payment.

## Endpoint

```http
GET /akash/market/v1beta4/leases/info
```

## Query Parameters

| Parameter   | Type   | Description         |
| ----------- | ------ | ------------------- |
| id.owner    | string | Owner               |
| id.dseq     | string | Deployment sequence |
| id.gseq     | string | Group sequence      |
| id.oseq     | string | Order sequence      |
| id.provider | string | Provider address    |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/market/v1beta4/leases/info?id.owner=akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p&id.dseq=12345678&id.gseq=1&id.oseq=1&id.provider=akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
```
{% endcode %}

## Response

```json
{
    "lease": {
        "lease_id": {
            "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "dseq": "12345678",
            "provider": "akashvaloper1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p"
        },
        "state": "active",
        "price": {
            "denom": "uakt",
            "amount": "1.5"
        }
    },
    "escrow_payment": {
        "state": "open",
        "balance": {
            "denom": "uakt",
            "amount": "5000000"
        }
    }
}
```

## Response Fields

| Field           | Type   | Description                  |
| --------------- | ------ | ---------------------------- |
| lease           | object | Lease id, state, and price   |
| escrow\_payment | object | Escrow payment for the lease |

## Use Cases

* **Billing**: Read a lease and its payment

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
