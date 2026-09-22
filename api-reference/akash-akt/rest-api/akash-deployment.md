---
description: >-
  Example code for the akash/deployment/v1beta3/deployments/info REST method.
  Complete guide on how to use akash/deployment/v1beta3/deployments/info REST
  method in GetBlock Web3 documentation.
---

# /akash/deployment/v1beta3/deployments/info - Akash

Returns a single deployment by its owner and deployment sequence (dseq), including its groups (resource requirements) and escrow account.

## Endpoint

```http
GET /akash/deployment/v1beta3/deployments/info
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

curl "${AKASH_REST}akash/deployment/v1beta3/deployments/info?id.owner=akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p&id.dseq=12345678"
```
{% endcode %}

## Response

```json
{
    "deployment": {
        "deployment_id": {
            "owner": "akash1nl7dg3xj9j2y6q7z0v4w8c2b5d1f3g6h9k2m4p",
            "dseq": "12345678"
        },
        "state": "active"
    },
    "groups": [
        {
            "group_id": {
                "gseq": 1
            },
            "state": "open"
        }
    ],
    "escrow_account": {
        "balance": {
            "denom": "uakt",
            "amount": "5000000"
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
