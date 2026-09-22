---
description: >-
  Example code for the akash/market/v1beta5/leases/list REST method. Complete
  guide on how to use akash/market/v1beta5/leases/list REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta5/leases/list - Akash

Returns leases — accepted bids that bind a deployment to a provider. A lease is an active rental of compute from a provider.

## Endpoint

```http
GET /akash/market/v1beta5/leases/list
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

curl "${AKASH_REST}akash/market/v1beta5/leases/list?pagination.limit=1"
```
{% endcode %}

## Response

```json
{
    "leases": [
        {
            "lease": {
                "id": {
                    "owner": "akash102kdwsssl6jf4frdcz50mua6j0h6d8wdhjh7nq",
                    "dseq": "25880666",
                    "gseq": 1,
                    "oseq": 1,
                    "provider": "akash19zzh7whjt4vfwxd5wtj3tjtyatnpntfhldshd8",
                    "bseq": 0
                },
                "state": "active",
                "price": {
                    "denom": "uact",
                    "amount": "4.826603000000000000"
                },
                "created_at": "25880690",
                "closed_on": "0",
                "reason": "lease_closed_invalid",
                "reclamation": null
            },
            "escrow_payment": {
                "id": {
                    "aid": {
                        "scope": "deployment",
                        "xid": "akash102kdwsssl6jf4frdcz50mua6j0h6d8wdhjh7nq/25880666"
                    },
                    "xid": "1/1/akash19zzh7whjt4vfwxd5wtj3tjtyatnpntfhldshd8"
                },
                "state": {
                    "owner": "akash19zzh7whjt4vfwxd5wtj3tjtyatnpntfhldshd8",
                    "state": "open",
                    "rate": {
                        "denom": "uact",
                        "amount": "4.826603000000000000"
                    },
                    "balance": {
                        "denom": "uact",
                        "amount": "0.000000000000000000"
                    },
                    "unsettled": {
                        "denom": "uact",
                        "amount": "0.000000000000000000"
                    },
                    "withdrawn": {
                        "denom": "uact",
                        "amount": "13780746"
                    }
                }
            }
        }
    ],
    "pagination": {
        "next_key": "qpP0SwQBAgMEAQFuYWthc2gxMDhlM202YXBxa2hjZzh4dHA3YW1tOTI0eTMybHhrZG45c3VuODcAAAAB...",
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
