---
description: >-
  Example code for the akash/market/v1beta5/leases/info REST method. Complete
  guide on how to use akash/market/v1beta5/leases/info REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta5/leases/info - Akash

Returns a single lease by its full id, plus its escrow payment.

## Endpoint

```http
GET /akash/market/v1beta5/leases/info
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

curl "${AKASH_REST}akash/market/v1beta5/leases/info?id.owner=akash102kdwsssl6jf4frdcz50mua6j0h6d8wdhjh7nq&id.dseq=25880666&id.gseq=1&id.oseq=1&id.provider=akash19zzh7whjt4vfwxd5wtj3tjtyatnpntfhldshd8"
```
{% endcode %}

## Response

```json
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
