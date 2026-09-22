---
description: >-
  Example code for the akash/deployment/v1beta3/deployments/list REST method.
  Complete guide on how to use akash/deployment/v1beta3/deployments/list REST
  method in GetBlock Web3 documentation.
---

# /akash/deployment/v1beta3/deployments/list - Akash

Returns Akash deployments, filterable by owner and state. A deployment is a user's request for cloud compute resources, described by a manifest and funded via escrow.

## Endpoint

```
GET /akash/deployment/v1beta3/deployments/list
```

## Query Parameters

| Parameter        | Type   | Description                       |
| ---------------- | ------ | --------------------------------- |
| filters.owner    | string | Filter by owner address (akash1…) |
| filters.state    | string | active or closed                  |
| pagination.limit | string | Max results                       |

## Example

{% code overflow="wrap" %}
```bash
export AKASH_REST=https://shared.eu-central-1.getblock.io/<ACCESS-TOKEN>/

curl "${AKASH_REST}akash/deployment/v1beta3/deployments/list"
```
{% endcode %}

## Response

```json
{
    "deployments": [
        {
            "deployment": {
                "deployment_id": {
                    "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
                    "dseq": "12345678"
                },
                "state": "active"
            },
            "escrow_account": {
                "balance": {
                    "denom": "uakt",
                    "amount": "5000000"
                }
            }
        }
    ],
    "pagination": {
        "total": "1"
    }
}
```

## Response Fields

| Field                                    | Type   | Description                                    |
| ---------------------------------------- | ------ | ---------------------------------------------- |
| deployments                              | array  | Deployments with id, state, and escrow account |
| deployments\[].deployment.deployment\_id | object | Owner + dseq identifying the deployment        |
| deployments\[].escrow\_account           | object | Escrow balance funding the deployment          |

## Use Cases

* **Marketplace UIs**: List a user's deployments
* **Monitoring**: Track active deployments and escrow
* **Indexing**: Ingest deployment state

## Error Handling

| Error                     | Message       | Description                                       |
| ------------------------- | ------------- | ------------------------------------------------- |
| 400 / Bad request         | Bad request   | A parameter was invalid                           |
| 403 / RBAC: access denied | Access denied | The GetBlock access token is missing or incorrect |
