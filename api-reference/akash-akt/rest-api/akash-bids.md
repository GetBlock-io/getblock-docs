---
description: >-
  Example code for the akash/market/v1beta5/bids/list REST method. Complete
  guide on how to use akash/market/v1beta5/bids/list REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta5/bids/list - Akash

Returns provider bids on marketplace orders. A bid is a provider's price offer to host a deployment's workload.

## Endpoint

```
GET /akash/market/v1beta5/bids/list
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

curl "${AKASH_REST}akash/market/v1beta5/bids/list?pagination.limit=1"
```
{% endcode %}

## Response

```json
{
    "bids": [
        {
            "bid": {
                "id": {
                    "owner": "akash1008l639687lnkcscsrha36n5zdt3qzz30yzc67",
                    "dseq": "22804430",
                    "gseq": 1,
                    "oseq": 1,
                    "provider": "akash13va9yxc8a7wlc882g72uj4g2fscj93hdk0uycp",
                    "bseq": 0
                },
                "state": "open",
                "price": {
                    "denom": "ibc/170C677610AC31DF0904FFE09CD3B5C657492170E7E52372E48756B71E56F2F1",
                    "amount": "2.526488000000000000"
                },
                "created_at": "25930029",
                "resources_offer": [
                    {
                        "resources": {
                            "id": 1,
                            "cpu": {
                                "units": {
                                    "val": "100"
                                },
                                "attributes": []
                            },
                            "memory": {
                                "quantity": {
                                    "val": "536870912"
                                },
                                "attributes": []
                            },
                            "storage": [
                                {
                                    "name": "default",
                                    "quantity": {
                                        "val": "1073741824"
                                    },
                                    "attributes": []
                                }
                            ],
                            "gpu": {
                                "units": {
                                    "val": "0"
                                },
                                "attributes": []
                            },
                            "endpoints": [
                                {
                                    "kind": "RANDOM_PORT",
                                    "sequence_number": 0
                                }
                            ]
                        },
                        "count": 1,
                        "prices": null
                    }
                ],
                "reclamation_window": null
            },
            "escrow_account": {
                "id": {
                    "scope": "bid",
                    "xid": "akash1008l639687lnkcscsrha36n5zdt3qzz30yzc67/22804430/1/1/akash13va9yxc8a7wlc882g72uj4g2fscj93hdk0uycp"
                },
                "state": {
                    "owner": "akash13va9yxc8a7wlc882g72uj4g2fscj93hdk0uycp",
                    "state": "open",
                    "transferred": [
                        {
                            "denom": "uakt",
                            "amount": "0.000000000000000000"
                        }
                    ],
                    "settled_at": "25930029",
                    "funds": [
                        {
                            "denom": "uakt",
                            "amount": "500000.000000000000000000"
                        }
                    ],
                    "deposits": [
                        {
                            "owner": "akash13va9yxc8a7wlc882g72uj4g2fscj93hdk0uycp",
                            "height": "25930029",
                            "source": "balance",
                            "balance": {
                                "denom": "uakt",
                                "amount": "500000.000000000000000000"
                            }
                        }
                    ]
                }
            }
        }
    ],
    "pagination": {
        "next_key": "VXGVlgQBAgMEAQFuYWthc2gxMDA4bDYzOTY4N2xua2NzY3NyaGEzNm41emR0M3F6ejMweXpjNjcAAAAA...",
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
