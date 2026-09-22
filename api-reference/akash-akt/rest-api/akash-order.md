---
description: >-
  Example code for the akash/market/v1beta5/orders/info REST method. Complete
  guide on how to use akash/market/v1beta5/orders/info REST method in GetBlock
  Web3 documentation.
---

# /akash/market/v1beta5/orders/info - Akash

Returns a single marketplace order by its full id (owner, dseq, gseq, oseq).

## Endpoint

```http
GET /akash/market/v1beta5/orders/info
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

curl "${AKASH_REST}akash/market/v1beta5/orders/info?id.owner=akash1000dryw6pj53kxt6fe8admaue5lvjf8jjprqqh&id.dseq=20805457&id.gseq=1&id.oseq=1"
```
{% endcode %}

## Response

```json
{
    "order": {
        "id": {
            "owner": "akash1000dryw6pj53kxt6fe8admaue5lvjf8jjprqqh",
            "dseq": "20805457",
            "gseq": 1,
            "oseq": 1
        },
        "state": "open",
        "spec": {
            "name": "akash",
            "requirements": {
                "signed_by": {
                    "all_of": [
                        "akash1365yvmc4s7awdyj3n2sav7xfx76adc6dnmlx63"
                    ],
                    "any_of": []
                },
                "attributes": [
                    {
                        "key": "console/trials",
                        "value": "true"
                    }
                ]
            },
            "resources": [
                {
                    "resource": {
                        "id": 1,
                        "cpu": {
                            "units": {
                                "val": "2000"
                            },
                            "attributes": []
                        },
                        "memory": {
                            "quantity": {
                                "val": "12000000000"
                            },
                            "attributes": []
                        },
                        "storage": [
                            {
                                "name": "default",
                                "quantity": {
                                    "val": "12884901888"
                                },
                                "attributes": []
                            }
                        ],
                        "gpu": {
                            "units": {
                                "val": "1"
                            },
                            "attributes": [
                                {
                                    "key": "vendor/nvidia/model/*",
                                    "value": "true"
                                }
                            ]
                        },
                        "endpoints": [
                            {
                                "kind": "RANDOM_PORT",
                                "sequence_number": 0
                            }
                        ]
                    },
                    "count": 1,
                    "price": {
                        "denom": "ibc/170C677610AC31DF0904FFE09CD3B5C657492170E7E52372E48756B71E56F2F1",
                        "amount": "10000.000000000000000000"
                    }
                }
            ]
        },
        "created_at": "20805459",
        "reclamation": null
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
