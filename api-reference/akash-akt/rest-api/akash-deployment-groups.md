---
description: >-
  Example code for the akash/deployment/v1beta4/groups/info REST method.
  Complete guide on how to use akash/deployment/v1beta4/groups/info REST
  method in GetBlock Web3 documentation.
---

# /akash/deployment/v1beta4/groups/info - Akash

Returns a single deployment group (a set of resource requirements within a deployment) by its owner, dseq, and gseq.

## Endpoint

```http
GET /akash/deployment/v1beta4/groups/info
```

## Query Parameters

| Parameter | Type   | Description         |
| --------- | ------ | ------------------- |
| id.owner  | string | Owner address       |
| id.dseq   | string | Deployment sequence |
| id.gseq   | string | Group sequence      |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/deployment/v1beta4/groups/info?id.owner=akash100dwtg4hqnd240x583spjwnk4kanp559xwtvmg&id.dseq=16122570&id.gseq=1"
```
{% endcode %}

## Response

```json
{
    "group": {
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
}
```

## Response Fields

| Field | Type   | Description                        |
| ----- | ------ | ---------------------------------- |
| group | object | Group id, state, and resource spec |

## Use Cases

* **Resource Detail**: Inspect a deployment's resource group

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
