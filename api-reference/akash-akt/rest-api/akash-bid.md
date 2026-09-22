---
description: >-
  Example code for the akash/market/v1beta5/bids/info REST method. Complete
  guide on how to use akash/market/v1beta5/bids/info REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta5/bids/info - Akash

Returns a single provider bid by its full id.

## Endpoint

```
GET /akash/market/v1beta5/bids/info
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

curl "${AKASH_REST}akash/market/v1beta5/bids/info?id.owner=akash1008l639687lnkcscsrha36n5zdt3qzz30yzc67&id.dseq=22804430&id.gseq=1&id.oseq=1&id.provider=akash13va9yxc8a7wlc882g72uj4g2fscj93hdk0uycp"
```
{% endcode %}

## Response

```json
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
```

## Response Fields

| Field           | Type   | Description              |
| --------------- | ------ | ------------------------ |
| bid             | object | Bid id, state, and price |
| escrow\_account | object | Bid escrow balance       |

## Use Cases

* **Price Discovery**: Inspect a specific bid

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
