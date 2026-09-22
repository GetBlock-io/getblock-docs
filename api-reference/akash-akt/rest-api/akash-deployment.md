---
description: >-
  Example code for the akash/deployment/v1beta4/deployments/info REST method.
  Complete guide on how to use akash/deployment/v1beta4/deployments/info REST
  method in GetBlock Web3 documentation.
---

# /akash/deployment/v1beta4/deployments/info - Akash

Returns a single deployment by its owner and deployment sequence (dseq), including its groups (resource requirements) and escrow account.

## Endpoint

```http
GET /akash/deployment/v1beta4/deployments/info
```

## Query Parameters

| Parameter | Type   | Description                        |
| --------- | ------ | ---------------------------------- |
| id.owner  | string | Deployment owner address (akash1…) |
| id.dseq   | string | Deployment sequence number         |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/deployment/v1beta4/deployments/info?id.owner=akash100dwtg4hqnd240x583spjwnk4kanp559xwtvmg&id.dseq=16122570"
```
{% endcode %}

## Response

```json
{
    "deployment": {
        "id": {
            "owner": "akash100dwtg4hqnd240x583spjwnk4kanp559xwtvmg",
            "dseq": "16122570"
        },
        "state": "active",
        "hash": "bLTCo5xFV2obtovLJ/rUZDHLkzAbB8vlXpF2iJGKpaY=",
        "created_at": "16122572",
        "reclamation": null
    },
    "groups": [
        {
            "id": {
                "owner": "akash100dwtg4hqnd240x583spjwnk4kanp559xwtvmg",
                "dseq": "16122570",
                "gseq": 1
            },
            "state": "open",
            "group_spec": {
                "name": "dcloud",
                "requirements": {
                    "signed_by": {
                        "all_of": [],
                        "any_of": []
                    },
                    "attributes": []
                },
                "resources": [
                    {
                        "resource": {
                            "id": 1,
                            "cpu": {
                                "units": {
                                    "val": "500"
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
                                        "val": "536870912"
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
                                    "kind": "SHARED_HTTP",
                                    "sequence_number": 0
                                }
                            ]
                        },
                        "count": 1,
                        "price": {
                            "denom": "uact",
                            "amount": "584.635140000000000000"
                        }
                    }
                ]
            },
            "created_at": "16122572"
        }
    ],
    "escrow_account": {
        "id": {
            "scope": "deployment",
            "xid": "akash100dwtg4hqnd240x583spjwnk4kanp559xwtvmg/16122570"
        },
        "state": {
            "owner": "akash100dwtg4hqnd240x583spjwnk4kanp559xwtvmg",
            "state": "open",
            "transferred": [
                {
                    "denom": "uakt",
                    "amount": "0.000000000000000000"
                }
            ],
            "settled_at": "16122572",
            "funds": [
                {
                    "denom": "uact",
                    "amount": "292317.570000000000000000"
                }
            ],
            "deposits": [
                {
                    "owner": "akash100dwtg4hqnd240x583spjwnk4kanp559xwtvmg",
                    "height": "0",
                    "source": "balance",
                    "balance": {
                        "denom": "uact",
                        "amount": "292317.570000000000000000"
                    }
                }
            ]
        }
    }
}
```

## Response Fields

| Field           | Type   | Description               |
| --------------- | ------ | ------------------------- |
| deployment      | object | Deployment id and state   |
| groups          | array  | Resource groups requested |
| escrow\_account | object | Escrow balance            |

## Use Cases

* **Deployment Detail**: Render a deployment page
* **Resource Inspection**: Read the requested compute groups
* **Escrow**: Check the funding balance

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
